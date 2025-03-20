---
title: "<div>Automating Agile workflows with the gitlab-triage gem</div>"
date: 2025-03-19
tags: 
  - "development"
  - "git"
  - "github"
  - "gitlab"
  - "software"
---

_Welcome to our "Getting started with GitLab" series, where we help newcomers get familiar with the GitLab DevSecOps platform._

This post dives into the `gitlab-triage` gem, a powerful tool that lets you create bots to automate your Agile workflow. Say goodbye to manual tasks and hello to streamlined efficiency.

## Why automate your workflow?

Efficiency is key in software development. Automating repetitive tasks like triaging issues and merge requests frees up valuable time for your team to focus on what matters most: building amazing software.

With `gitlab-triage`, you can:

- **Ensure consistency:** Apply labels and assign issues automatically based on predefined rules.
- **Improve response times:** Get immediate feedback on new issues and merge requests.
- **Reduce manual effort:** Eliminate the need for manual triage and updates.
- **Boost productivity:** Free up your team to focus on coding and innovation.

## Introducing the `gitlab-triage` gem

The `gitlab-triage` gem is a Ruby library that allows you to create bots that interact with your GitLab projects. These bots can automatically perform a wide range of actions, including:

- **Labeling:** Automatically categorize issues and merge requests.
- **Commenting:** Provide updates, request information, or give feedback.
- **Assigning:** Assign issues and merge requests to the appropriate team members.
- **Closing:** Close stale or resolved issues and merge requests.
- **Creating:** Generate new issues based on specific events or conditions.
- **And much more!**

Check out the `gitlab-triage` gem repository.

## Setting up your triage bot

Let's get your first triage bot up and running!

1. Install the gem. (Note: The gem command is available with Ruby programming language installed.)

```bash
gem install gitlab-triage
```

2. Get your GitLab API token.

- Go to your GitLab profile settings.
- Navigate to **Access Tokens**.
- Create a new token with the `api` scope.
- **Keep your token secure and set an expiration date for it based on when you will be done with this walkthrough!**

3. Define your triage policies.

Create a file named `.triage-policies.yml` in your project's root directory. This file will contain the rules that govern your bot's behavior. Here's a simple example:

```yaml

---
- name: "Apply 'WIP' label"
  condition:
    draft: true
  action:
    labels:
      - status::wip

- name: "Request more information on old issue"
  condition:
   date:
    attribute: updated_at
    condition: older_than
    interval_type: months
    interval: 12
  action:
    comment: |
      {GitLab} This issue has been open for more than 12 months, is this still an issue?
```

This configuration defines two policies:

- The first policy applies the `status::wip` label to any issue that is in draft.
- The second policy adds a comment to an issue that the issue has not been updated in 12 months.

4. Run your bot.

You can run your bot manually using the following command:

```bash
gitlab-triage -t <your_api_token> -p <your_project_id>
```

Replace `<your_api_token>` with your GitLab API token and `<your_project_id>` with the ID of your GitLab project. If you would like to see the impact of actions before they are taken, you can add the `-n` or `--dry-run` to test out the policies first.

## Automating with GitLab CI/CD

To automate the execution of your triage bot, integrate it with GitLab CI/CD. Here's an example `.gitlab-ci.yml` configuration:

```yaml

triage:
  script:
    - gem install gitlab-triage
    - gitlab-triage -t $GITLAB_TOKEN -p $CI_PROJECT_ID
  only:
    - schedules
```

This configuration defines a job named "triage" that installs the `gitlab-triage` gem and runs the bot using the `$GITLAB_TOKEN` (a predefined CI/CD variable) and the `$CI_PROJECT_ID` variable. The `only: schedules` clause ensures that the job runs only on a schedule.

To create a schedule, go to your project's **CI/CD** settings and navigate to **Schedules**. Create a new schedule and define the frequency at which you want your bot to run (e.g., daily, hourly).

## Advanced triage policies

`gitlab-triage` offers a range of advanced features for creating more complex triage policies:

- **Regular expressions:** Use regular expressions for more powerful pattern matching.
- **Summary policies:** Consolidate related issues into a single summary issue.
- **Custom actions:** Define custom actions using Ruby code blocks to perform more complex operations using the GitLab API.

Here are two advanced real-world examples from the triage bot used by the Developer Advocacy team at GitLab. You can view the full policies in this file.

```yaml
- name: Issues where DA team member is an assignee outside DA-Meta project i.e. DevRel-Influenced
  conditions:
    assignee_member:
      source: group
      condition: member_of
      source_id: 1008
    state: opened
    ruby: get_project_id != 18 
    forbidden_labels:
      - developer-advocacy
  actions:   
    labels:
      - developer-advocacy
      - DevRel-Influenced
      - DA-Bot::Skip
```

This example for issues across a group, excluding those in the project with the ID of 18, have assignees who are members of the group with ID of 1008 and do not have the label `developer-advocacy` on them. This policy helps the Developer Advocacy team at GitLab to find issues members of the team are assigned to but are not in their team’s project. This helps the team identify and keep track of contributions made outside of the team by adding the teams’ labels.

```
- name: Missing Due Dates
  conditions:
    ruby: missing_due_date
    state: opened
    labels:
      - developer-advocacy
    forbidden_labels:
      - DA-Due::N/A
      - DA-Bot::Skip
      - DA-Status::FYI
      - DA-Status::OnHold
      - CFP
      - DA-Bot::Triage
  actions:
    labels:
      - DA-Bot-Auto-Due-Date
    comment: |
      /due #{get_current_quarter_last_date}
```

This second example checks for all issues with the `developer-advocacy` label, which do not include labels in the forbidden labels list and when their due dates have passed. It updates the due dates automatically by commenting on the issue with a slash command and a date that is generated using Ruby.

The Ruby scripts used in the policies are defined in a separate file as shown below. This feature allows you to be flexible in working with your filters and actions. You can see functions are created for different Ruby commands that we used in our policies.

```
require 'json'
require 'date'
require "faraday"
require 'dotenv/load'

module DATriagePlugin
  def last_comment_at
    conn = Faraday.new(
      url: notes_url+"?sort=desc&order_by=created_at&pagination=keyset&per_page=1",
      headers: {'PRIVATE-TOKEN' => ENV.fetch("PRIV_KEY"), 'Content-Type' => 'application/json' }
    )

    response = conn.get()
    if response.status == 200
      jsonData = JSON.parse(response.body)
      if jsonData.length > 0
        Date.parse(jsonData[0]['created_at'])
      else
        Date.parse(resource[:created_at])
      end
    else
      Date.parse(resource[:created_at])
    end
  end

  def notes_url
    resource[:_links][:notes]
  end

  def get_project_id
    resource[:project_id]
  end

  def get_current_quarter_last_date()
    yr = Time.now.year
    case Time.now.month
    when 2..4
      lm = 4
    when 5..7
      lm = 7
    when 8..10
      lm = 10
    when 11..12
      lm = 1
      yr = yr + 1
    else
      lm = 1    
    end

    return Date.new(yr, lm, -1) 
  end

  def one_week_to_due_date
    if(resource[:due_date] == nil)
      false
    else
      days_to_due = (Date.parse(resource[:due_date]) - Date.today).to_i
      if(days_to_due > 0 && days_to_due < 7)
        true
      else
        false
      end
    end
  end

  def due_date_past
    if(resource[:due_date] == nil)
      false
    else
      Date.today > Date.parse(resource[:due_date])
    end
  end

  def missing_due_date
    if(resource[:due_date] == nil)
      true
    else
      false
    end
  end

end

Gitlab::Triage::Resource::Context.include DATriagePlugin

```

The triage bot is executed using the command:

```
`gitlab-triage -r ./triage_bot/issue_triage_plugin.rb --debug --token $PRIV_KEY --source-id gitlab-com --source groups`  
```

- `-r`: Passes in a file of requirements for the performing triage. In this case we are passing in our Ruby functions.
- `--debug`: Prints debugging information as part of the output.
- `--token`: Is used to pass in a valid GitLab API token.
- `--source`: Specifies if the sources of the issues it will search is within a group or a project.
- `--source-id`: Takes in the ID of the selected source type – in this case, a group.

The GitLab triage-ops project is another another real-world example that is more complex and you can learn how to build your own triage bot.

## Best practices

- **Start simple:** Begin with basic policies and gradually increase complexity as needed.
- **Test thoroughly:** Test your policies in a staging environment before deploying them to production.
- **Monitor regularly:** Monitor your bot's activity to ensure it's behaving as expected.
- **Use descriptive names:** Give your policies clear and descriptive names for easy maintenance.
- **Be mindful of the scope of your filters:** You might be tempted to filter issues across groups where thousands of issues exist. However, this can slow down the triage and also make the process fail due to rate limitations against the GitLab API.
- **Prioritize using labels for triages:** To avoid spamming other users, labels are a good way to perform triages without cluttering comments and issues.

## Take control of your workflow

With the `gitlab-triage` gem, you can automate your GitLab workflow and unlock new levels of efficiency. Start by creating simple triage bots and gradually explore the more advanced features. You'll be amazed at how much time and effort you can save!

## Getting started with GitLab series

Check out more articles in our "Getting Started with GitLab" series below:

- How to manage users
    
- How to import your projects to GitLab
    
- Mastering project management
    

_Welcome to our "Getting started with GitLab" series, where we help newcomers get familiar with the GitLab DevSecOps platform._

This post dives into the `gitlab-triage` gem, a powerful tool that lets you create bots to automate your Agile workflow. Say goodbye to manual tasks and hello to streamlined efficiency.

## Why automate your workflow?

Efficiency is key in software development. Automating repetitive tasks like triaging issues and merge requests frees up valuable time for your team to focus on what matters most: building amazing software.

With `gitlab-triage`, you can:

- **Ensure consistency:** Apply labels and assign issues automatically based on predefined rules.
- **Improve response times:** Get immediate feedback on new issues and merge requests.
- **Reduce manual effort:** Eliminate the need for manual triage and updates.
- **Boost productivity:** Free up your team to focus on coding and innovation.

## Introducing the `gitlab-triage` gem

The `gitlab-triage` gem is a Ruby library that allows you to create bots that interact with your GitLab projects. These bots can automatically perform a wide range of actions, including:

- **Labeling:** Automatically categorize issues and merge requests.
- **Commenting:** Provide updates, request information, or give feedback.
- **Assigning:** Assign issues and merge requests to the appropriate team members.
- **Closing:** Close stale or resolved issues and merge requests.
- **Creating:** Generate new issues based on specific events or conditions.
- **And much more!**

Check out the `gitlab-triage` gem repository.

## Setting up your triage bot

Let's get your first triage bot up and running!

1. Install the gem. (Note: The gem command is available with Ruby programming language installed.)

```bash
gem install gitlab-triage
```

2. Get your GitLab API token.

- Go to your GitLab profile settings.
- Navigate to **Access Tokens**.
- Create a new token with the `api` scope.
- **Keep your token secure and set an expiration date for it based on when you will be done with this walkthrough!**

3. Define your triage policies.

Create a file named `.triage-policies.yml` in your project's root directory. This file will contain the rules that govern your bot's behavior. Here's a simple example:

```yaml

---
- name: "Apply 'WIP' label"
  condition:
    draft: true
  action:
    labels:
      - status::wip

- name: "Request more information on old issue"
  condition:
   date:
    attribute: updated_at
    condition: older_than
    interval_type: months
    interval: 12
  action:
    comment: |
      {GitLab} This issue has been open for more than 12 months, is this still an issue?
```

This configuration defines two policies:

- The first policy applies the `status::wip` label to any issue that is in draft.
- The second policy adds a comment to an issue that the issue has not been updated in 12 months.

4. Run your bot.

You can run your bot manually using the following command:

```bash
gitlab-triage -t <your_api_token> -p <your_project_id>
```

Replace `<your_api_token>` with your GitLab API token and `<your_project_id>` with the ID of your GitLab project. If you would like to see the impact of actions before they are taken, you can add the `-n` or `--dry-run` to test out the policies first.

## Automating with GitLab CI/CD

To automate the execution of your triage bot, integrate it with GitLab CI/CD. Here's an example `.gitlab-ci.yml` configuration:

```yaml

triage:
  script:
    - gem install gitlab-triage
    - gitlab-triage -t $GITLAB_TOKEN -p $CI_PROJECT_ID
  only:
    - schedules
```

This configuration defines a job named "triage" that installs the `gitlab-triage` gem and runs the bot using the `$GITLAB_TOKEN` (a predefined CI/CD variable) and the `$CI_PROJECT_ID` variable. The `only: schedules` clause ensures that the job runs only on a schedule.

To create a schedule, go to your project's **CI/CD** settings and navigate to **Schedules**. Create a new schedule and define the frequency at which you want your bot to run (e.g., daily, hourly).

## Advanced triage policies

`gitlab-triage` offers a range of advanced features for creating more complex triage policies:

- **Regular expressions:** Use regular expressions for more powerful pattern matching.
- **Summary policies:** Consolidate related issues into a single summary issue.
- **Custom actions:** Define custom actions using Ruby code blocks to perform more complex operations using the GitLab API.

Here are two advanced real-world examples from the triage bot used by the Developer Advocacy team at GitLab. You can view the full policies in this file.

```yaml
- name: Issues where DA team member is an assignee outside DA-Meta project i.e. DevRel-Influenced
  conditions:
    assignee_member:
      source: group
      condition: member_of
      source_id: 1008
    state: opened
    ruby: get_project_id != 18 
    forbidden_labels:
      - developer-advocacy
  actions:   
    labels:
      - developer-advocacy
      - DevRel-Influenced
      - DA-Bot::Skip
```

This example for issues across a group, excluding those in the project with the ID of 18, have assignees who are members of the group with ID of 1008 and do not have the label `developer-advocacy` on them. This policy helps the Developer Advocacy team at GitLab to find issues members of the team are assigned to but are not in their team’s project. This helps the team identify and keep track of contributions made outside of the team by adding the teams’ labels.

```
- name: Missing Due Dates
  conditions:
    ruby: missing_due_date
    state: opened
    labels:
      - developer-advocacy
    forbidden_labels:
      - DA-Due::N/A
      - DA-Bot::Skip
      - DA-Status::FYI
      - DA-Status::OnHold
      - CFP
      - DA-Bot::Triage
  actions:
    labels:
      - DA-Bot-Auto-Due-Date
    comment: |
      /due #{get_current_quarter_last_date}
```

This second example checks for all issues with the `developer-advocacy` label, which do not include labels in the forbidden labels list and when their due dates have passed. It updates the due dates automatically by commenting on the issue with a slash command and a date that is generated using Ruby.

The Ruby scripts used in the policies are defined in a separate file as shown below. This feature allows you to be flexible in working with your filters and actions. You can see functions are created for different Ruby commands that we used in our policies.

```
require 'json'
require 'date'
require "faraday"
require 'dotenv/load'

module DATriagePlugin
  def last_comment_at
    conn = Faraday.new(
      url: notes_url+"?sort=desc&order_by=created_at&pagination=keyset&per_page=1",
      headers: {'PRIVATE-TOKEN' => ENV.fetch("PRIV_KEY"), 'Content-Type' => 'application/json' }
    )

    response = conn.get()
    if response.status == 200
      jsonData = JSON.parse(response.body)
      if jsonData.length > 0
        Date.parse(jsonData[0]['created_at'])
      else
        Date.parse(resource[:created_at])
      end
    else
      Date.parse(resource[:created_at])
    end
  end

  def notes_url
    resource[:_links][:notes]
  end

  def get_project_id
    resource[:project_id]
  end

  def get_current_quarter_last_date()
    yr = Time.now.year
    case Time.now.month
    when 2..4
      lm = 4
    when 5..7
      lm = 7
    when 8..10
      lm = 10
    when 11..12
      lm = 1
      yr = yr + 1
    else
      lm = 1    
    end

    return Date.new(yr, lm, -1) 
  end

  def one_week_to_due_date
    if(resource[:due_date] == nil)
      false
    else
      days_to_due = (Date.parse(resource[:due_date]) - Date.today).to_i
      if(days_to_due > 0 && days_to_due < 7)
        true
      else
        false
      end
    end
  end

  def due_date_past
    if(resource[:due_date] == nil)
      false
    else
      Date.today > Date.parse(resource[:due_date])
    end
  end

  def missing_due_date
    if(resource[:due_date] == nil)
      true
    else
      false
    end
  end

end

Gitlab::Triage::Resource::Context.include DATriagePlugin

```

The triage bot is executed using the command:

```
`gitlab-triage -r ./triage_bot/issue_triage_plugin.rb --debug --token $PRIV_KEY --source-id gitlab-com --source groups`  
```

- `-r`: Passes in a file of requirements for the performing triage. In this case we are passing in our Ruby functions.
- `--debug`: Prints debugging information as part of the output.
- `--token`: Is used to pass in a valid GitLab API token.
- `--source`: Specifies if the sources of the issues it will search is within a group or a project.
- `--source-id`: Takes in the ID of the selected source type – in this case, a group.

The GitLab triage-ops project is another another real-world example that is more complex and you can learn how to build your own triage bot.

## Best practices

- **Start simple:** Begin with basic policies and gradually increase complexity as needed.
- **Test thoroughly:** Test your policies in a staging environment before deploying them to production.
- **Monitor regularly:** Monitor your bot's activity to ensure it's behaving as expected.
- **Use descriptive names:** Give your policies clear and descriptive names for easy maintenance.
- **Be mindful of the scope of your filters:** You might be tempted to filter issues across groups where thousands of issues exist. However, this can slow down the triage and also make the process fail due to rate limitations against the GitLab API.
- **Prioritize using labels for triages:** To avoid spamming other users, labels are a good way to perform triages without cluttering comments and issues.

## Take control of your workflow

With the `gitlab-triage` gem, you can automate your GitLab workflow and unlock new levels of efficiency. Start by creating simple triage bots and gradually explore the more advanced features. You'll be amazed at how much time and effort you can save!

## Getting started with GitLab series

Check out more articles in our "Getting Started with GitLab" series below:

- How to manage users
    
- How to import your projects to GitLab
    
- Mastering project management
    

Go to Source

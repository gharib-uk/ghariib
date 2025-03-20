---
title: "Building an AI Article Reviewer using GenAI Platform and GitHub"
date: 2025-03-17T00:00:00.000Z
draft: false
type: posts
categories: 
- gen-ai,genai-platform,ai-ml
---
# Building an AI Article Reviewer using GenAI Platform and GitHub

<br/>

<br/>
### [Introduction](#introduction)[](#introduction)

If you write for a living or as a hobby, you might have noticed that while personal writing can follow any structure, professional writing needs to adhere to certain style and writing guides and undergo certain reviews before being published.

This tutorial is for those with GitHub-based workflows and a desire to speed-up the review process. You will learn to implement a solution that leverages AI to accelerate the review process by creating a [GitHub action](https://github.com/features/actions) trained on your writing and style guides. This action will check for issues, identify them and suggest how you can improve it.

![Image 1](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/AI-reviewer-GH-action/Screenshot%202025-03-10%20at%2011.30.05%E2%80%AFAM.png)

[Prerequisites](#prerequisites)[](#prerequisites)
-------------------------------------------------

-   Introductory knowledge on [yaml](https://yaml.org/) to build the [GitHub Action](https://github.com/features/actions).
-   Working knowledge of [JavaScript](/community/tutorial-series/how-to-code-in-javascript).
-   A [DigitalOcean account](http://cloud.digitalocean.com) to use the [GenAI platform](/products/gen-ai) to train the GitHub Action accordingly.

[The problem and how to solve it?](#the-problem-and-how-to-solve-it)[](#the-problem-and-how-to-solve-it)
--------------------------------------------------------------------------------------------------------

Reviews take time and are usually manual with some checks in place. Manual reviews can take longer when the workload is heavy, adding to the backlog.

You will build a GitHub action that uses **AI trained on your technical guides and documentation**. It learns your company’s or team’s writing style and then suggests changes accordingly. This significantly reduces the review time from a few days to a few seconds.

This is how the GitHub action works:

![Image 2](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/AI-reviewer-GH-action/Screenshot%202025-03-10%20at%2011.30.26%E2%80%AFAM.png)

The GitHub action is already [available on the marketplace](https://github.com/marketplace/actions/markdown-grammar-checker), so you do not need to build it from scratch. You can directly use it on your repositories, trained on your data.

This will involve two main steps:

1.  Deploying a [DigitalOcean GenAI agent](/products/gen-ai) trained on your tutorials.
2.  Adding the workflow file to your repositories to use the GitHub action.

[Building the DigitalOcean GenAI agent](#building-the-digitalocean-genai-agent)[](#building-the-digitalocean-genai-agent)
-------------------------------------------------------------------------------------------------------------------------

In this step, we will see how we can build the [GenAI agent](/community/conceptual-articles/integrate-gen-ai-agents) that reviews the markdown files you commit and gives suggestions based on your writing style.

You will need to add and index a knowledge base to ensure the agent is trained on your tutorials.

-   Once you have logged in to [DigitalOcean](https://cloud.digitalocean.com/), go to the [`GenAI platform`](https://cloud.digitalocean.com/gen-ai/welcome?i=0a3f9f), and from the `Knowledge bases` tab, click on `Create Knowledge Base`.  
    ![Image 3](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/AI-reviewer-GH-action/Screenshot%202025-03-10%20at%2011.30.38%E2%80%AFAM.png)
-   Give it a name, and from `Select data source`, choose `URL for web crawling`. Now, depending on where your tutorials or documentation are located and how many levels you want to crawl, you can choose Scoped/URL and all linked pages in path/URL and all linked pages in domain/Subdomains.
    -   For this example, we will choose the `Subdomains` option since it is the broadest option and can lead to better results. You may or may not choose to index embedded media.  
        ![Image 4](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/AI-reviewer-GH-action/Screenshot%202025-03-10%20at%2011.30.47%E2%80%AFAM.png)
-   Then, choose an embedding model of your choice and click on `Create knowledge base`.

Now that we have created a knowledge base, we will move ahead with creating an agent and referencing the knowledge base to the agent. For that:

-   Go to the `GenAI Platform` option on the left pane and click `Create agent`.  
    ![Image 5](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/AI-reviewer-GH-action/Screenshot%202025-03-10%20at%2011.31.00%E2%80%AFAM.png)
-   Give it a unique name, and write a prompt something similar to:

```
Your task is to check for grammatical errors in an article. Use the knowledge base to learn how the writing style is and make sure to check for these three things:
- Look for typos, punctuation errors, and incorrect sentence structures.
- Use active voice and avoid passive constructions where possible.
- Check for H tags, and trailing spaces.
```

-   Select a model of your choice. In this example, we use [`Claude 3.5 Sonnet`](https://www.anthropic.com/news/claude-3-5-sonnet) and select the knowledge base we created in the first step.  
    ![Image 6](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/AI-reviewer-GH-action/Screenshot%202025-03-10%20at%2011.31.09%E2%80%AFAM.png)
-   Finally, click on `Create Agent`.
-   This will take a few minutes. Once deployed, go to `Playground` and paste a markdown file with incorrect grammar and structure that does not follow your writing style. You will see that the agent is responding to you with issues and suggestions, as shown in the image below.  
    ![Image 7](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/AI-reviewer-GH-action/Screenshot%202025-03-10%20at%2011.31.23%E2%80%AFAM.png)

And that’s how you have completed training and building an AI agent. This was the first and the most essential step.

[Using the GitHub Action](#using-the-github-action)[](#using-the-github-action)
-------------------------------------------------------------------------------

Now that the AI agent is created, the next step is to use the [GitHub Action](https://github.com/marketplace/actions/markdown-grammar-checker/) on your repositories.

1\. Copy the agent endpoint, and from the `Settings` tab, create a key and keep it copied. These are needed to access the GenAI agent.  
![Image 8](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/AI-reviewer-GH-action/Screenshot%202025-03-10%20at%2011.31.54%E2%80%AFAM.png) ![Image 9](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/AI-reviewer-GH-action/Screenshot%202025-03-10%20at%2011.32.03%E2%80%AFAM.png)  
2\. Then, go to your repository. From the `Settings` tab, under `Actions`, add the endpoint and key, as shown in the screenshot below.  
![Image 10](https://doimages.nyc3.cdn.digitaloceanspaces.com/006Community/AI-reviewer-GH-action/Screenshot%202025-03-10%20at%2011.32.15%E2%80%AFAM.png)

3\. Then, in your repository, in the workflow folder, create a `.yml` file (e.g., `.github/workflows/grammar-check.yml`)and paste the following code:

```
name: Check Markdown Grammar

on:
  pull_request:
    types: [opened, synchronize, reopened]
    paths:
      - '**.md'
  workflow_dispatch:

jobs:
  check-markdown:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
        with:
          fetch-depth: 0  
      - name: Get changed files
        id: changed-files
        uses: dorny/paths-filter@v2
        with:
          filters: |
            markdown:
              - '**/*.md'
              - '!**/node_modules/**'
      
      - name: Check Markdown Grammar
        if: steps.changed-files.outputs.markdown == 'true'
        uses: Haimantika/article-review-github-action@v1.1.1
        with:
          do-api-token: ${{ secrets.DO_API_TOKEN }}
          do-agent-base-url: ${{ secrets.DO_AGENT_BASE_URL }}
          file-pattern: ${{ steps.changed-files.outputs.files_markdown }}
          exclude-pattern: '**/node_modules/**,**/vendor/**'
```

4\. To test the action, open a [pull request](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request) with a markdown file, and you will see it running the check. It will fail if there are any issues; if not, it will pass and be ready to be merged.

[Building your own GitHub Action](#building-your-own-github-action)[](#building-your-own-github-action)
-------------------------------------------------------------------------------------------------------

The first step is to write a workflow file that automatically triggers the action when a pull request is opened. It checks the grammar and writing style of the markdown file using a `grammar-checker.js` script.

This is how the code will look:

```
name: Markdown Grammar Checker

on:
  pull_request:
    types: [opened, synchronize, reopened]
    paths:
      - '**.md'
  workflow_dispatch:

jobs:
  grammar-check:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v3
        with:
          fetch-depth: 0  

      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
          
      - name: Install dependencies
        run: npm install axios
          
      - name: Get changed files
        id: changed-files
        uses: dorny/paths-filter@v2
        with:
          base: ${{ github.event.pull_request.base.sha }}
          ref: ${{ github.event.pull_request.head.sha }}
          filters: |
            markdown:
              - '**.md'
              - '!**/node_modules/**'

      - name: Run grammar checks
        if: steps.changed-files.outputs.markdown == 'true'
        env:
          DO_API_TOKEN: ${{ secrets.DO_API_TOKEN }}
          DO_AGENT_BASE_URL: ${{ secrets.DO_AGENT_BASE_URL }}
        run: |
          echo "Running grammar checks on changed files..."
          
          # Split the files by newlines
          IFS=

#### [Source](https://www.digitalocean.com/community/tutorials/ai-article-reviewer-genai-platform-github)

<br/>
---
\n'
          files=($(echo "${{ steps.changed-files.outputs.files_markdown }}"))
          
          for file in "${files[@]}"; do
            echo "Checking grammar in $file..."
            node grammar-checker.js "$file"
          done
```

The next step is to write the script that scans the markdown file(s) raised in a pull request, extracts plain text from them, sends the text to DigitalOcean’s AI Agent for grammar checking, and reports any detected issues.

_You have already learned at the beginning of the article how to [create an AI agent](/community/conceptual-articles/integrate-gen-ai-agents) using DigitalOcean. So follow those steps._

The code for that will be:

```
#!/usr/bin/env node

const fs = require('fs');
const path = require('path');
const axios = require('axios');

// DigitalOcean AI Agent endpoint and key
const AGENT_BASE_URL = process.env.DO_AGENT_BASE_URL || 'https://agent-aaf74f4416df5696a67b-o4npv.ondigitalocean.app';  // Update this to your actual base URL
const AGENT_ENDPOINT = `${AGENT_BASE_URL}/api/v1/chat/completions`;
const API_TOKEN = process.env.DO_API_TOKEN;

if (!API_TOKEN) {
  console.error('Error: DigitalOcean API token not found. Please set the DO_API_TOKEN environment variable.');
  process.exit(1);
}

// Get all markdown files from the repository
const getAllMarkdownFiles = (dir, fileList = [], excludeDirs = ['node_modules', '.git']) => {
  const files = fs.readdirSync(dir);
  
  files.forEach(file => {
    const filePath = path.join(dir, file);
    
    // Skip excluded directories
    if (fs.statSync(filePath).isDirectory()) {
      if (!excludeDirs.includes(file)) {
        getAllMarkdownFiles(filePath, fileList, excludeDirs);
      }
    } else if (file.endsWith('.md')) {
      fileList.push(filePath);
    }
  });
  
  return fileList;
};

// Extract plain text content from markdown
const extractTextFromMarkdown = (content) => {
  // Remove YAML front matter
  let text = content;
  if (content.startsWith('---')) {
    const endOfFrontMatter = content.indexOf('---', 3);
    if (endOfFrontMatter !== -1) {
      text = content.slice(endOfFrontMatter + 3);
    }
  }
  
  // Remove code blocks
  text = text.replace(/```[\s\S]*?```/g, '');
  
  // Remove HTML tags
  text = text.replace(/<[^>]*>/g, '');
  
  // Remove markdown links but keep the text
  text = text.replace(/\[([^\]]+)\]\([^)]+\)/g, '$1');
  
  // Remove images
  text = text.replace(/!\[[^\]]*\]\([^)]+\)/g, '');
  
  return text;
};

// Function to check grammar using DigitalOcean's AI Agent
const checkGrammar = async (text) => {
  try {
    console.log("Sending request to DigitalOcean AI agent...");
    
    const response = await axios.post(AGENT_ENDPOINT, {
      model: "claude-3.5-sonnet",
      messages: [
        {
          role: "system",
          content: "You are a skilled editor focused on identifying grammatical errors, typos, incorrect sentence structures, passive voice, and unnecessary jargon."
        },
        {
          role: "user",
          content: `Please review the following text for grammatical errors, typos, incorrect sentence structures, passive voice, and unnecessary jargon. For each issue, identify the specific problem, explain why it's an issue, and suggest a correction. Format your response as a JSON array with objects containing: "issue_type", "text_with_issue", "explanation", and "suggestion". Only identify actual issues. If there are no grammatical problems, return an empty array.\n\nText to review:\n${text}`
        }
      ],
      temperature: 0.0,
      max_tokens: 1024
    }, {
      headers: {
        'Content-Type': 'application/json',
        'Authorization': `Bearer ${API_TOKEN}`
      }
    });
    
    console.log("Received response from AI agent");
    
    // Parse the AI response to get the JSON data
    const aiResponse = response.data.choices[0].message.content;
    try {
      // Extract JSON from the response (in case there's additional text)
      const jsonMatch = aiResponse.match(/\[[\s\S]*\]/);
      return jsonMatch ? JSON.parse(jsonMatch[0]) : [];
    } catch (e) {
      console.error('❌ Error parsing AI response:', e);
      console.log('AI response:', aiResponse);
      return [];
    }
  } catch (error) {
    console.error('❌ Error checking grammar:', error.message);
    if (error.response) {
      console.error('Response status:', error.response.status);
      console.error('Response data:', JSON.stringify(error.response.data, null, 2));
    }
    return [];
  }
};

// Process a single markdown file
const processFile = async (filePath) => {
  try {
    console.log(`\nChecking grammar in ${filePath}...`);
    
    // Check if file exists
    if (!fs.existsSync(filePath)) {
      console.error(`❌ Error: File does not exist: ${filePath}`);
      return false;
    }
    
    const content = fs.readFileSync(filePath, 'utf8');
    const textToCheck = extractTextFromMarkdown(content);
    
    // Skip empty files or files with very little text content
    if (textToCheck.trim().length < 50) {
      console.log(`⚠️ Skipping ${filePath}: Not enough text content to check`);
      return true;
    }
    
    console.log(`Sending content to grammar check API...`);
    
    const issues = await checkGrammar(textToCheck);
    
    if (issues.length === 0) {
      console.log(`✅ ${filePath}: No grammar issues found`);
      return true;
    } else {
      console.log(`⚠️ ${filePath}: Found ${issues.length} grammar issues:`);
      issues.forEach((issue, index) => {
        console.log(`  ${index + 1}. ${issue.issue_type}: "${issue.text_with_issue}"`);
        console.log(`     Explanation: ${issue.explanation}`);
        console.log(`     Suggestion: ${issue.suggestion}`);
        console.log();
      });
      return false;
    }
  } catch (error) {
    console.error(`❌ Error processing ${filePath}:`, error.message);
    return false;
  }
};

// Main function to process all markdown files
const checkAllFiles = async () => {
  // Get files to check - either from command line args or find all
  let markdownFiles = [];
  
  if (process.argv.length > 2) {
    // Use files passed as arguments
    markdownFiles = process.argv.slice(2);
    console.log(`Checking grammar in specific file(s): ${markdownFiles.join(', ')}`);
  } else {
    // Find all markdown files
    markdownFiles = getAllMarkdownFiles('.');
    console.log(`Found ${markdownFiles.length} markdown files to check for grammar`);
  }
  
  if (markdownFiles.length === 0) {
    console.log('No files to check.');
    return true;
  }
  
  let allValid = true;
  
  // Process each file
  for (const file of markdownFiles) {
    const fileValid = await processFile(file);
    if (!fileValid) {
      allValid = false;
    }
  }
  
  return allValid;
};

// Run the grammar checker
checkAllFiles().then(allValid => {
  if (!allValid) {
    console.error('\n❌ Grammar check failed: Issues were found');
    process.exit(1);
  } else {
    console.log('\n✅ Grammar check passed: No issues were found');
    process.exit(0);
  }
}).catch(error => {
  console.error('❌ Error running grammar check:', error.message);
  process.exit(1);
});
```

[Making the action ready to be used by the Community](#making-the-action-ready-to-be-used-by-the-community)[](#making-the-action-ready-to-be-used-by-the-community)
-------------------------------------------------------------------------------------------------------------------------------------------------------------------

Before your action is ready to be used by the public, you need to test it locally and ensure it works as expected.

Once that is done, create an `action.yml` file that defines your action’s metadata:

```
name: 'Markdown Grammar Checker'
description: 'Checks markdown files for grammar, style, and formatting issues using AI'
author: 'Your Name'

inputs:
  github-token:
    description: 'GitHub token for accessing PR files'
    required: true
    default: ${{ github.token }}
  do-api-token:
    description: 'DigitalOcean API token'
    required: true
  do-agent-base-url:
    description: 'DigitalOcean AI agent base URL'
    required: true
  file-pattern:
    description: 'Glob pattern for files to check'
    required: false
    default: '**/*.md'
  exclude-pattern:
    description: 'Glob pattern for files to exclude'
    required: false
    default: '**/node_modules/**'

runs:
  using: 'node16'
  main: 'index.js'

branding:
  icon: 'book'
  color: 'blue'
```

-   Then, create an [`index.js`](https://github.com/Haimantika/article-review-github-action/blob/main/index.js) file that works with the GitHub Actions toolkit.
-   Create a [`package.json`](https://github.com/Haimantika/article-review-github-action/blob/main/package.json) file that contains all the requirements.
-   Once everything is done, you can make a release and [publish it on marketplace](https://docs.github.com/en/actions/sharing-automations/creating-actions/publishing-actions-in-github-marketplace).
-   Then, finally, add the `yml` file that users need to add to their workflow file to use your action. It will look something like this:

```
name: Check Markdown Grammar

on:
  pull_request:
    types: [opened, synchronize, reopened]
    paths:
      - '**.md'
  workflow_dispatch:

jobs:
  check-markdown:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
        with:
          fetch-depth: 0  
      - name: Get changed files
        id: changed-files
        uses: dorny/paths-filter@v2
        with:
          filters: |
            markdown:
              - '**/*.md'
              - '!**/node_modules/**'
      
      - name: Check Markdown Grammar
        if: steps.changed-files.outputs.markdown == 'true'
        uses: Haimantika/article-review-github-action@v1.1.1
        with:
          do-api-token: ${{ secrets.DO_API_TOKEN }}
          do-agent-base-url: ${{ secrets.DO_AGENT_BASE_URL }}
          file-pattern: ${{ steps.changed-files.outputs.files_markdown }}
          exclude-pattern: '**/node_modules/**,**/vendor/**'
```

And that’s how you can build your GitHub action!

[Conclusion](#conclusion)[](#conclusion)
----------------------------------------

In this tutorial, you learned how to build a comprehensive AI article reviewer system using [DigitalOcean’s GenAI Platform](/products/gen-ai) and [GitHub Actions](https://docs.github.com/en/actions). The system is designed to streamline technical writing review processes by leveraging AI capabilities to check for grammatical errors, suggest improvements, and ensure consistency with your team’s writing style.

If you would like to learn more about DigitalOcean, make sure to follow these resources:

-   [DigitalOcean Documentation](https://docs.digitalocean.com/).
-   [DigitalOcean YouTube](https://www.youtube.com/@DigitalOcean).
-   [DigitalOcean Discord Community](https://discord.gg/digitalocean).

#### [Source](https://www.digitalocean.com/community/tutorials/ai-article-reviewer-genai-platform-github)

<br/>
---

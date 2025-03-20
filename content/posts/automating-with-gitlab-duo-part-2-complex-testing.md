---
title: "<div>Automating with GitLab Duo, Part 2: Complex testing</div>"
date: 2025-01-03
categories: 
  - "general"
tags: 
  - "development"
  - "git"
  - "github"
  - "gitlab"
  - "software"
---

The first part of our three-part series on test generation with GitLab Duo focused on how to automate code testing. Now, we will share the lessons we learned while using AI for test generation.

## Situations we encountered and how we handled them

Overall, we were pleased with the results using GitLab Duo to generate tests on our code. As is the case with any language generation, some cases required minor adjustments such as fixing import paths or editing contents in datasets. For the more complex cases, we had to remember that AI solutions often lack context. Here's how we handled the more complex testing situations with GitLab Duo.

### Updating existing test cases

As is often the case when developing a software product, we encountered instances that required updates to existing tests. Rather than manually making adjustments to a full test suite for a common issue, we took full advantage of the GitLab Duo Chat window in VS Code. For example, to refactor tests, we used the Chat prompt “Please update the provided tests to use unittest rather than pytest” followed by pasting in the tests we wanted GitLab Duo to update.

![Automated test generation](https://images.ctfassets.net/r9o86ar0p03f/eSDIEGqi7Wz45wWjqjWeE/d082b52e703eba3142164f8f2d203c11/image5.png)

<br></br>

![Chat prompt requesting use of unittest rather than pytest](https://images.ctfassets.net/r9o86ar0p03f/6H0edPtZPc5TpoS0q4vhG6/b37a7eb3e1de35c05efd907b395e88f4/image3.png)

**Note:** We copy-and-pasted GitLab Duo's recommendations into our code.

### Creating tests for legacy code

Creating tests for legacy code we knew worked was another challenging situation we encountered. In such circumstances, it was valuable to provide error snippets alongside failing tests and ask GitLab Duo to provide new tests. A full copy-and-paste from the terminal window of noted failures and errors to Chat, along with a request to “Please explain and fix this failing test” or similar prompts, yielded a summary of the issues the test was encountering as well as a new test addressing the problem. We did find this sometimes required multiple rounds of refactoring as new test failures were identified. However, the efficiency of GitLab Duo to provide various refactored solutions was fast and a net positive on team and developer efficiency.

### Working with complex or abstracted code

In other instances, the modularization or complexity of our code led to variance in GitLab Duo’s results. For instance, when generating tests, GitLab Duo sometimes generated a series of passing and failing tests caused by differences in testing approach (e.g. usage of Mock and which objects were mocked). We provided GitLab Duo its own example of a passing test and asked it to modify individual tests one at a time to match the style of the passing tests to maintain consistency. We also would provide GitLab Duo a file of functioning tests for a similar object or task so it could mirror the structure.

### Ensuring generated code follows our standards

While developing a Python module, GitLab Duo generated many tests using Mock and often they required refactoring, particularly around naming standardization. In such cases, we could leverage GitLab Duo Chat to refactor tests with instructions as to which specific test components to update. Prompting GitLab Duo for these changes was immensely faster than refactoring tests individually, as we had previousy done.

### Addressing uncovered test cases

GitLab Duo generated tests for additional test cases the team had not previously considered, thus increasing coverage. Luckily, we could use GitLab Duo to quickly and efficiently address these edge cases and expand testing coverage, which is a key value-add for our team to build quickly and ensure a robust product.

## What we learned

Here are few key lessons that have been important to our success with GitLab Duo:

- **Fast and efficient for rapid development and iteration -** GitLab Duo’s role in generating automated tests has been a key accelerator in development for our team and allowed us to work faster and with greater confidence in our changes.
- **Important to use appropriate prompts -** When using GitLab Duo for our use case, we touched on a key topic for machine learning optimization: prompt engineering. Sometimes we needed to modify our question by just a few keywords to lead to the ideal generated answer.
- **Need understanding of underlying frameworks and code -** When it comes to any AI-generated code that makes it into a product, even if only as testing, it’s critical that we understand how the code functions so we can adequately debug as well as request informed changes.
- **Need understanding of desired end state and standards -** Similar to following coding standards for formatting and library usage while developing without AI, it’s important to maintain the vision of what the intended outcomes look like and what standards are being adhered to when using AI. GitLab Duo needs the context to understand code standards, so it’s critical for team members using GitLab Duo to provide adequate oversight of its outputs to ensure quality and other expectations are met.
- **GitLab Duo is not a replacement for all tests -** While we use GitLab Duo significantly for generating automated tests, it does not replace our other tests and human oversight. Functional tests, integration tests, and more still serve a valuable place in the QA process and overall software development lifecycle.

In our next article in this series, we’ll cover a test we ran to validate the impact of GitLab Duo on our team’s automated testing and discuss the impressive results we have achieved thus far.

The first part of our three-part series on test generation with GitLab Duo focused on how to automate code testing. Now, we will share the lessons we learned while using AI for test generation.

## Situations we encountered and how we handled them

Overall, we were pleased with the results using GitLab Duo to generate tests on our code. As is the case with any language generation, some cases required minor adjustments such as fixing import paths or editing contents in datasets. For the more complex cases, we had to remember that AI solutions often lack context. Here's how we handled the more complex testing situations with GitLab Duo.

### Updating existing test cases

As is often the case when developing a software product, we encountered instances that required updates to existing tests. Rather than manually making adjustments to a full test suite for a common issue, we took full advantage of the GitLab Duo Chat window in VS Code. For example, to refactor tests, we used the Chat prompt “Please update the provided tests to use unittest rather than pytest” followed by pasting in the tests we wanted GitLab Duo to update.

![Automated test generation](https://images.ctfassets.net/r9o86ar0p03f/eSDIEGqi7Wz45wWjqjWeE/d082b52e703eba3142164f8f2d203c11/image5.png)

<br></br>

![Chat prompt requesting use of unittest rather than pytest](https://images.ctfassets.net/r9o86ar0p03f/6H0edPtZPc5TpoS0q4vhG6/b37a7eb3e1de35c05efd907b395e88f4/image3.png)

**Note:** We copy-and-pasted GitLab Duo's recommendations into our code.

### Creating tests for legacy code

Creating tests for legacy code we knew worked was another challenging situation we encountered. In such circumstances, it was valuable to provide error snippets alongside failing tests and ask GitLab Duo to provide new tests. A full copy-and-paste from the terminal window of noted failures and errors to Chat, along with a request to “Please explain and fix this failing test” or similar prompts, yielded a summary of the issues the test was encountering as well as a new test addressing the problem. We did find this sometimes required multiple rounds of refactoring as new test failures were identified. However, the efficiency of GitLab Duo to provide various refactored solutions was fast and a net positive on team and developer efficiency.

### Working with complex or abstracted code

In other instances, the modularization or complexity of our code led to variance in GitLab Duo’s results. For instance, when generating tests, GitLab Duo sometimes generated a series of passing and failing tests caused by differences in testing approach (e.g. usage of Mock and which objects were mocked). We provided GitLab Duo its own example of a passing test and asked it to modify individual tests one at a time to match the style of the passing tests to maintain consistency. We also would provide GitLab Duo a file of functioning tests for a similar object or task so it could mirror the structure.

### Ensuring generated code follows our standards

While developing a Python module, GitLab Duo generated many tests using Mock and often they required refactoring, particularly around naming standardization. In such cases, we could leverage GitLab Duo Chat to refactor tests with instructions as to which specific test components to update. Prompting GitLab Duo for these changes was immensely faster than refactoring tests individually, as we had previousy done.

### Addressing uncovered test cases

GitLab Duo generated tests for additional test cases the team had not previously considered, thus increasing coverage. Luckily, we could use GitLab Duo to quickly and efficiently address these edge cases and expand testing coverage, which is a key value-add for our team to build quickly and ensure a robust product.

## What we learned

Here are few key lessons that have been important to our success with GitLab Duo:

- **Fast and efficient for rapid development and iteration -** GitLab Duo’s role in generating automated tests has been a key accelerator in development for our team and allowed us to work faster and with greater confidence in our changes.
- **Important to use appropriate prompts -** When using GitLab Duo for our use case, we touched on a key topic for machine learning optimization: prompt engineering. Sometimes we needed to modify our question by just a few keywords to lead to the ideal generated answer.
- **Need understanding of underlying frameworks and code -** When it comes to any AI-generated code that makes it into a product, even if only as testing, it’s critical that we understand how the code functions so we can adequately debug as well as request informed changes.
- **Need understanding of desired end state and standards -** Similar to following coding standards for formatting and library usage while developing without AI, it’s important to maintain the vision of what the intended outcomes look like and what standards are being adhered to when using AI. GitLab Duo needs the context to understand code standards, so it’s critical for team members using GitLab Duo to provide adequate oversight of its outputs to ensure quality and other expectations are met.
- **GitLab Duo is not a replacement for all tests -** While we use GitLab Duo significantly for generating automated tests, it does not replace our other tests and human oversight. Functional tests, integration tests, and more still serve a valuable place in the QA process and overall software development lifecycle.

In our next article in this series, we’ll cover a test we ran to validate the impact of GitLab Duo on our team’s automated testing and discuss the impressive results we have achieved thus far.

Go to Source

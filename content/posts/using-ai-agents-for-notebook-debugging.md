---
title: "Using AI Agents for Notebook Debugging"
date: 2025-03-19
categories: 
  - "development"
tags: 
  - "dev"
  - "developers"
  - "development"
  - "jetbrains"
  - "linux"
  - "software"
---

Debugging computational notebooks can be quite frustrating for a variety of reasons, including issues like out-of-order cell execution, missing data files, and library conflicts. Traditional AI tools often struggle with these problems due to the interactive nature of notebooks, but at JetBrains, we are exploring new solutions. Our research team has developed a prototype of \[…\]

Debugging computational notebooks can be quite frustrating for a variety of reasons, including issues like out-of-order cell execution, missing data files, and library conflicts. Traditional AI tools often struggle with these problems due to the interactive nature of notebooks, but at JetBrains, we are exploring new solutions.

Our research team has developed a prototype of an AI agent that autonomously fixes notebook errors by modifying and executing cells until the problems are solved. 

In this post, we’ll explore how the prototype works, how it improves the debugging process, and how it compares to simpler LLM-based solutions in terms of cost and user experience.

### Why automate notebook debugging

Computational notebooks are widely used for data analysis, research, and hypothesis testing because they allow users to integrate different data types and execute cells non-linearly. This creates vast opportunities for experimentation and use in data science.

However, this flexibility and interactivity come at the cost of reproducibility and having to deal with frequent bugs. Studies show that up to 75% of Jupyter notebooks cannot be re-run without issues. At the same time, debugging them is particularly difficult, as it requires the interactive, iterative inspection of errors and their context rather than simple static analysis.

Using LLMs like GPT-4 is standard practice for other debugging tasks. However, for notebook debugging, it is rather challenging due to context size limitations. Notebooks require runtime information to reflect their state, which means they often need a large context window for error resolution. AI agents address this by interacting with notebooks iteratively – much like humans do, but significantly faster.

The JetBrains Research team has created a prototype of just such an AI agent for Datalore. It is capable of creating, editing, and executing cells to resolve runtime exceptions.

Here’s a short demo showcasing the debugging AI agent in action:

<iframe loading="lazy" title="Demo. Debug Smarter, Not Harder: AI Agents for Error Resolution in Computational Notebooks" width="500" height="281" src="https://www.youtube.com/embed/kOGQO2Gtvs4?feature=oembed" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

If you want to see a detailed report on this project, check out this preprint. These results were also presented at EMNLP, a leading conference for natural language processing and artificial intelligence.

### How does the debugging AI agent work?

## ![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdMy8MtqAeNSwdUEAfJIn3909M_Cau0OgmzK8tzVJ9lWnIW1kDF3jRoH0HTTUIeYtHEfUNn8o84RMp86ONNqIG9F5hV5n2-juNclunNBMasCnXy5bwLlvdtA5tvP3G2Fdrmwdk6?key=6COu_-nzVoNQpnuO_cPEwP-y)

Let’s start with a broad overview of our AI agent system. It consists of three main components – the LLM module, the environment, and the user interface – which work together as follows:

1. **Start:** Whenever the execution of a cell results in an exception, the debugging AI agent can be called with a _Fix with AI Agent_ button that appears below the error message. When clicked, it starts the agentic debugging cycle, which is shown as an interaction with an LLM in a separate window. 

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXeaJSh6Eb0VZC75i70WOEK08H2FkPZHkQjZJ4IHoduR9CZjknYa1blnX7OW_l6MCwSvoJvjQ2xYq4X-QSNTu5jXepUHVXQA-fRIipdAxSAtCuVn6nqJYcHJ3yr79Qb2UnJTR5PPqA?key=6COu_-nzVoNQpnuO_cPEwP-y)

2. **Cycle**: At each iteration of the debugging cycle, the LLM module collects the required information and adds it to the debugging prompt passed to the LLM, in this case, GPT-4. The LLM then generates an answer, which includes some reasoning as well as the next action, such as editing and executing cells or finishing the debugging process once the error has been resolved. The action suggested by the model is then executed in the environment, and the results are added to the prompt for the next round of debugging. 

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdAsQKC_SeTP9aDCRyZKeN0z9E5IyylCLmsvPp6UK4-wsma3UPXvLUn1Pu7o4PZUQuBa1jtBlScboc3Y2uDfR88M1aTiC9GBtoheY0tvUlqCAS_ovHxTtsdrzMpB-6yFbPa5x97?key=6COu_-nzVoNQpnuO_cPEwP-y)

3. **Finish**: This iterative loop continues until the error is resolved or until the limit on time or the number of iterations is reached. In our experiments, most of the debugging runs ended in the error being successfully resolved after only a few steps, so the limits were rarely required in practice.

### How does the AI agent prompt the LLM?

Let’s take a closer look at the core of our AI agent system, namely, the LLM module. Our AI agent prompts the LLM using a “system prompt” that covers the general approach to the task and a “user prompt” containing specific details about the current stage of the debugging run. Below, you can see shortened versions of the two prompts.

| System prompt |  |
| --- | --- |
| General purpose | You are a coding assistant that should help solve the user’s error in a computational notebook. You should use functions to help handle real-time user queries and return outputs only in the form of valid JSON. |
| Available tools | You have a few ways of interacting with the environment:   change the code of the existing cells, run it, and give the output.add a new cell with your own code, run it, and give the output.run any cell as is and give the output.If you’re sure the error won’t show up in the cell where it was found, you can run finish. |
| Workflow guidelines | Keep trying for at least 10 steps before you stop, but if you think you’ve solved the problem, you can run finish right away.If you can fix the error without changing any code, do that. Don’t edit the existing code or add new code unless you really need to.Use only the functions given to you. If you have many functions to choose from, pick the one that solves the problem quickest.When you think you’ve fixed the problem, run finish to say you’re done. |
| Hacks | Just adding try – except is not a solution. Commenting out the code that produced the error is not a solution. You should propose only meaningful solutions.  |

| Initial user prompt | Here’s a Jupyter notebook. It uses {separator} as a separator between cells. Note that cell indexes START FROM 1! {notebook} An error occurred in the cell with num {cell\_num}. The error trace is the following: {error} Please resolve the error. You must use only the tools defined above to resolve the error. Return output only as valid JSON.  |
| --- | --- |

The system prompt opens with a general description of the problem, provides some guidelines on how it should be solved, and then closes with instructions about actions. We use reflection as the algorithm for choosing the next action, asking the LLM to reflect on the outcomes of the previous actions taken before selecting the next tool to call. The reflections are output as comments for each selected action and are available at all following steps. The guidelines also encourage the LLM to explore the environment and discourage large outputs and “hacks”, such as deleting the error cell or commenting out the problematic code. 

The user prompt initially consists of the notebook code and the error cell number, again accompanied by a short task description. After each step, the actions suggested by the LLM are converted into commands and executed, and the result is collected as an input for the next user prompt, along with the conversation history.

You can observe the entire debugging process in real time via the interface where the conversation and the actions are visualized.

## How costly is the AI agent?

A significant cost with any AI system comes from running large language models, with costs primarily driven by the number of request and response tokens processed.

To compare costs, we evaluated the AI agent against a simpler zero-shot debugging approach in Datalore, using 100 hours of hackathon coding records. The dataset featured an extensive notebook action history, so the errors that occurred could be reproduced and addressed by either of the AI solutions. On average, resolving an error with the AI agent was 2.5 times as expensive, costing $0.22 per error with GPT-4 versus $0.09 with the simpler method.

The AI agent’s higher cost came mainly from using three times as many request tokens for its memory stack, while the number of response tokens was similar between methods. Nevertheless, the cost remained practical for industry use since input tokens are cheaper and most errors were fixed on the first step.

## User feedback about the AI agent

We used the same zero-shot baseline to compare the user experiences with these two AI algorithms. In particular, we wanted to know how well users thought the AI agent resolved problems and how much they believed it could improve their productivity. We had two groups of programmers complete the same data-filtering task. One group used the zero-shot baseline, and the other used our agentic solution.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfZXbDn8nc2dSsGutdNKxMZMTaX8yssBpNM8iPP3bJW5MxtoYYMDloUhpsAxziXYmefeain2mNUfTkfBQ7lj78nqs7wyKWrrVJbnkXoJOKDxDZMJ8PoAbezLpkJmeQ34pw3bUEf?key=6COu_-nzVoNQpnuO_cPEwP-y)

We found that the users were quite satisfied with both solutions, rating them either good or excellent, and they slightly favored the AI agent. The users also seemed to rely more on the AI agent. However, the participants also reported that the agentic solution was less easy to use. We suspect this may be due to the lack of user control over its actions as well as the speed at which it acts. 

In the feedback, the users suggested that slowing down the conversation and highlighting the introduced changes in the notebook would provide more insight into the AI agent’s thought process and solution. They also proposed making the agentic solution process more interactive so they have a way of interrupting or changing the actions suggested by the AI agent. Finally, they asked for a way to access the agent before any error has occurred.

## Benefits of AI debugging: Looking forward

Finally, we compared the AI agent to the unaided human debugging process that comprised our dataset. In this comparison, the agentic solution offers obvious benefits:

- **Faster debugging**: The AI agent typically fixed issues in under a minute, while humans took an average of 3.5 minutes and up to 20 minutes for more complex cases.

- **Effectiveness for common small errors**: The agent quickly resolved small bugs, like incorrect pandas functions or arguments.

- **Insights into root causes**: The agent highlighted and explained changes, which helped to identify root causes of and solutions for errors.

The main takeaways of the study for the agentic debugging research concern the areas of improvement:

- **Reduced cost**: One solution to the higher cost of the agentic approach could be history caching, which can reduce the number of request tokens used. 

- **Increased user control**: The study of user experience highlighted that users would prefer to have more control over the process, such as the option to call the agent before any error occurs. 

Building on top of this work, we will continue making Datalore smarter with AI-based features.

Go to Source

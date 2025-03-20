---
title: "How GitHub engineers learn new codebases"
date: 2025-03-19
tags: 
  - "development"
  - "git"
  - "github"
  - "gitlab"
  - "software"
---

Strategies to quickly get up to speed, whether you're a seasoned engineer or a newcomer to the field.

The post How GitHub engineers learn new codebases appeared first on The GitHub Blog.

No matter where you are in your coding career, you will likely come across a new codebase or problem domain that is completely unfamiliar to you. Because codebases can be filled with many layers of design patterns, bugfixes, and temporary workarounds, learning a new one can be a time-consuming and frustrating process.

Last year, I moved to a new team at GitHub. During my transition, I collected insights from colleagues about how they approach learning new technical spaces. A fascinating collection of strategies emerged, and I’m excited to share them!

Below are the most effective methods I gathered, organized by approach. Whether you’re a seasoned engineer switching teams or a newcomer to the field, these strategies can help make your next codebase onboarding a little bit easier.

## Hands-on code exploration

One of the best ways to get started is working directly with the code itself:

- **Start with “Good First Issues”**: Begin your journey by tackling smaller, well-defined tasks. These issues are often carefully selected by the team that owns the codebase to help newcomers understand key components without becoming overwhelmed. They provide natural entry points into the system, while delivering immediate value to the team.
- **Learn with GitHub Copilot**: Open up your Copilot Chat window next to the codebase as you’re exploring it. You can ask Copilot Chat your questions, and then use the `/explain` functionality for things that are difficult to understand. Learn more about using Copilot Chat in your development environment here. Here are some example queries that I have used with GitHub Copilot to enhance my understanding of a codebase:
    - What will this function return if I give it X?
    - Summarize what this method is made to do
    - What are some potential gaps in the existing tests for this method?
- **Analyze telemetry and metrics**: Modern applications generate vast amounts of performance and usage data. Study these metrics to understand how the system behaves in production, what patterns emerge during peak usage, and which components require the most attention. This data-driven approach provides invaluable context about the application’s real-world behavior.
- **Explore through testing**: Make deliberate modifications to the code and observe their effects. Write new tests to verify your understanding, and intentionally break things (in development) to see how the system fails. This helps build an intuitive understanding of the application’s boundaries and failure modes.

## Collaborative learning

Knowledge sharing is often the fastest path to understanding:

- **Pair program**: Don’t just observe—actively participate in pairing sessions with experienced team members. Ask questions about their workflow, note which files they frequently access, and get to know their debugging strategies. Even if you’re mainly watching, you’ll absorb valuable context about how different pieces fit together.
- **Understand the “why”**: When assigned tasks, dig deep into the motivation behind them. Understanding the business context and technical rationale helps you make better architectural decisions and aids in future problem solving. Don’t be afraid to ask seemingly basic questions. They often lead to important insights.
- **Monitor team communications**: Stay active in team chat channels and incident response discussions. Pay special attention to production alerts and how the team responds to them. This exposure helps you understand common failure patterns and builds muscle memory for handling incidents.

## Documentation and knowledge management

Writing and organizing information helps solidify understanding:

- **Create personal documentation**: Maintain a living document of your discoveries, questions, and insights. Document important code paths, architectural decisions, and system interfaces as you encounter them. This will become an invaluable reference and help identify gaps in your understanding.
- **Build technical maps**: Create diagrams of system architecture, data flows, and entity relationships. Start with high-level “black boxes” and gradually fill in the details as your understanding grows. Visual representations often reveal patterns and relationships that aren’t obvious in the code. One tool that I use for this is Figma. I start with adding the basic blocks that I understand about a system and how they interact, and then continuously zoom in and out on different parts of the system to add to the map as I learn.
- **Maintain a command cheat sheet**: Keep track of useful commands, scripts, and workflows you discover. Include context about when and why to use them. This becomes especially valuable when working with complex build systems or deployment pipelines. Here’s an example of a command cheat sheet that I often refer to for markdown syntax.
- **Gather information on the domain**: One key to managing your knowledge of a codebase is to have a deep understanding of the domain. This can be gained from product owners, customer insights, or from industry best practices if the domain is generalizable enough. Deeply understanding the domain and what customers in that space find the most critical are key to learning a new codebase.

## Learn by teaching

One great way to verify your understanding of a topic is the ability to accurately explain it to others. If you created personal documentation, as recommended in the previous section, you can formalize it into guides and official documentation for future new members of your team:

- **Write internal guides**: Once you learn something new, document it for the next person. This forces you to organize what you’ve learned and often reveals areas where your understanding isn’t as complete as you thought.
- **Contribute to official documentation**: When you find gaps in the existing documentation, take the initiative to improve it. This not only helps future team members but also validates your understanding with current experts. Learn more about writing documentation for your GitHub repository from our GitHub-flavored markdown guide.
- Regularly reflect on your learning by answering these key questions:
    - Can you describe the system in a few concise sentences?
    - How does it interact with adjacent systems?
    - What surprised you most during the learning process?
    - What aspects remain unclear?

After all of these recommendations, I’ve found that my favorite way to learn a new codebase is through documenting it, and turning that documentation into something that others can use in the future. Writing things down forces me to structure my thoughts and identify gaps in my understanding.

Here is a markdown template I developed for learning new codebases, which I use in conjunction with these methods to systematically build my knowledge.

> No matter how you learn, getting familiar with a new codebase can take some time. If you find that you need to brush up on your GitHub skills for any of these areas, check out this GitHub for Beginners video.  
> Happy learning!

The post How GitHub engineers learn new codebases appeared first on The GitHub Blog.

Go to Source

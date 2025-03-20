---
title: "Tools and DI(Dependency Injection) : Streamlining AI Development with PydanticAI"
date: 2025-01-15
---

AI applications are rapidly evolving, and with the right tools, building flexible and efficient systems has never been easier. One such powerful tool is **Dependency Injection (DI)**, a technique that allows you to pass dynamic data, configurations, or even database connections directly to your tools. This enhances the flexibility and scalability of your agents, making them more adaptable and powerful. With **PydanticAI**, DI is seamlessly integrated, making it simpler to handle and manipulate structured data.

In this blog post, we’ll dive into how we can leverage **PydanticAI**’s Dependency Injection, using a dynamic example that demonstrates how DI works with structured data via **Pydantic models**.

### Why Dependency Injection?

At its core, Dependency Injection is about decoupling your components. Instead of hardcoding data into your functions or agents, DI allows you to inject required data at runtime. This is especially beneficial for AI tools where data might include:

- **Passing State or Session**
- **Database connections**
- **Configuration settings**
- **User inputs or context-specific variables**

By passing these dependencies dynamically, you can make your tools modular, reusable, and adaptable, keeping your codebase clean and maintainable.

### Setting Up the Example: A Simple AI Calculator

Let’s create a calculator agent that can perform basic arithmetic operations (addition, subtraction, multiplication, division). This agent will take two numbers as input and dynamically decide the operation based on the query.

#### Step 1: Defining the Data Models

We'll start by defining two **Pydantic models**:

- **Request Model**: For passing two numbers (num1 and num2) to the agent.
- **Result Model**: For structuring the response, including the operation and the result.

```
from pydantic import BaseModel

class Request(BaseModel):
    num1: int
    num2: int

class Result(BaseModel):
    First_Number: int
    Second_Number: int
    Operation: str
    Answer: str
```

#### Step 2: Initializing the Agent

Using **PydanticAI**, we initialize the agent with a system prompt. We also specify the **Result** model as the result type, ensuring that the response is structured accordingly.  

```
from pydantic_ai import Agent
from dotenv import load_dotenv

load_dotenv()

agent = Agent(
    model="openai:gpt-3.5-turbo",
    system_prompt="You are a helpful calculator",
    result_type=Result
)
```

#### Step 3: Creating Tools with Dependency Injection

Now, we’ll define four tools (add, subtract, multiply, divide) for the calculator. Each tool accepts a **RunContext** object, which contains the injected data (the two numbers).  

```
from pydantic_ai import RunContext

@agent.tool
def add(ctx: RunContext[Request]) -> int:
    return ctx.deps.num1 + ctx.deps.num2

@agent.tool
def sub(ctx: RunContext[Request]) -> int:
    return ctx.deps.num1 - ctx.deps.num2

@agent.tool
def mul(ctx: RunContext[Request]) -> int:
    return ctx.deps.num1 * ctx.deps.num2

@agent.tool
def div(ctx: RunContext[Request]) -> int:
    return ctx.deps.num1 / ctx.deps.num2
```

Notice how we access `ctx.deps.num1` and `ctx.deps.num2`, which are dynamically injected from the **Request** model during runtime.

#### Step 4: Running the Agent

Finally, in the main block, we create an instance of the **Request** model, passing in the numbers to be used. This data is then injected into the agent’s **run\_sync** method.  

```
if __name__ == "__main__":
    deps = Request(num1=10, num2=10)
    result = agent.run_sync("What is 10 plus 10 and multiply by 5", deps=deps)
    print(result.data)
    print(result.data.model_dump_json()) # print JSON format 

```

## Output

![Image description](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F30tbr3ko3y0pqu86rx6g.png)

The agent parses the query, determines the necessary operations, and produces a structured response. The output includes the first number, second number, the operation performed, and the result — all as defined by the **Result** model.

### What Makes This Approach Powerful?

1. **Dynamic Data Injection**: With DI, you can pass any type of data (configurations, database handles, etc.) to the agent at runtime.
2. **Structured Results**: **Pydantic** ensures that responses are consistent and structured, making it easy to interpret.
3. **Reusable Tools**: Each tool is modular and independent, allowing you to reuse them in various contexts without redundancy.

### Conclusion

By integrating **PydanticAI** with **Dependency Injection**, creating AI-powered tools becomes more intuitive and scalable. Whether you're building simple utilities or complex workflows, this approach streamlines your code and enhances maintainability.

Ready to dive in? Try out this calculator example, and explore the powerful potential of Dependency Injection in AI applications!

If you have any questions or thoughts, feel free to drop them in the comments. Happy coding! 😊

**Thanks  
Sreeni Ramadorai**

Go to Source

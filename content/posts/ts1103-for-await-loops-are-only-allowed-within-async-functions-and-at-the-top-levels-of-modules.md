---
title: "<div>TS1103: 'for await' loops are only allowed within async functions and at the top levels of modules</div>"
date: 2025-01-15
---

# TS1103: 'for await' loops are only allowed within async functions and at the top levels of modules

TypeScript is a powerful programming language that builds on JavaScript by adding static types. It allows developers to specify the data types of variables, function parameters, and return values, which can help catch errors early in the development process. But what are types? In programming, types are classifications that specify what kind of value a variable can hold—like numbers, strings, or objects. TypeScript’s type system helps in defining these classifications, providing better tooling and error checking.

If you're eager to dive deeper into TypeScript or learn to code using AI tools like gpteach, consider subscribing or following my blog for more insightful content!

## Understanding 'for await' Loops in TypeScript

The error message TS1103: 'for await' loops are only allowed within async functions and at the top levels of modules arises due to inappropriate use of `for await...of` loops. Let's break this down.

### What are 'for await' Loops?

The `for await...of` loop is a special construct in TypeScript (and JavaScript) that allows you to asynchronously iterate over asynchronous iterable objects (e.g., Promises or async generators). This means each iteration can wait for a Promise to resolve before moving to the next iteration, which is very useful when you're dealing with sequences of asynchronous operations.

### Causes of TS1103 Error

The TS1103 error occurs when you attempt to use a `for await...of` loop outside of an asynchronous context. Specifically, you can only use this loop within:

1. An `async` function.
2. The top level of an ECMAScript module.

Let's look at a code example that would trigger this error:  

```
// This will cause TS1103
const asyncIterable = {
  [Symbol.asyncIterator]: async function* () {
    yield Promise.resolve(1);
    yield Promise.resolve(2);
  }
};

for await (const num of asyncIterable) { // Error: TS1103
  console.log(num);
}
```

### Fixing the Error

To resolve the TS1103 error, wrap your `for await...of` loop in an `async` function. Here's how you can fix it:  

```
const asyncIterable = {
  [Symbol.asyncIterator]: async function* () {
    yield Promise.resolve(1);
    yield Promise.resolve(2);
  }
};

async function processAsyncIterable() {
  for await (const num of asyncIterable) {
    console.log(num);
  }
}

processAsyncIterable(); // This works correctly!
```

By placing the loop inside the `async` function `processAsyncIterable`, you adhere to the rules dictated by the TS1103 constraint.

## Important Things to Know about TS1103

1. **Async Functions:** All code that uses `for await...of` needs to be inside an async function. Async functions are functions that return a Promise and can be awaited.
2. **Top-Level Statements:** You may use `for await...of` at the top level if your code is written in an ECMAScript module format (i.e., using `import` and `export`).
3. **Error Checking:** TypeScript's static type checking means that these errors can be caught at compile time, helping prevent runtime issues.

## FAQ's

**Q: What happens if I forget to use async for 'for await'?**  
  
A: You will encounter the TS1103 error, as the loop is embedded outside the required async context.

**Q: Can I use 'for await' in regular functions?**  
  
A: No, 'for await' loops can only be used in async functions or at the module level.

**Q: How do I create an async iterable object?**  
  
A: You can create an async iterable object by implementing the `Symbol.asyncIterator` method and yielding Promises.

In conclusion, understanding TS1103: 'for await' loops are only allowed within async functions and at the top levels of modules is crucial for writing effective and error-free asynchronous code in TypeScript. By adhering to these rules and organizing your code properly, you'll be able to leverage the power of asynchronous programming effectively.

Go to Source

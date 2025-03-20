---
title: "<div>TS1085: Octal literals are not available when targeting ECMAScript 5 and higher. Use the syntax '{0}'</div>"
date: 2025-01-15
tags: 
  - "prevents"
---

# TS1085: Octal literals are not available when targeting ECMAScript 5 and higher. Use the syntax '{0}'

TypeScript is a powerful programming language that builds on JavaScript by adding optional static types. It enhances JavaScript's capabilities, making it easier to write and manage larger codebases. In TypeScript, types (which are essentially categories of values) help developers ensure that their code behaves as expected by catching errors early in the development process.

For those new to programming, it's essential to understand that **types** specify the kind of data (like numbers, strings, or objects) that can be assigned to a variable. This prevents mistakes such as trying to perform operations on incompatible types, which can lead to bugs in your application.

If you want to continue your journey in TypeScript or learn to use AI tools like gpteach to enhance your coding skills, consider subscribing to my blog for more insightful articles!

## Understanding Superset Languages

TypeScript is often referred to as a superset of JavaScript. This means every valid JavaScript program is also a valid TypeScript program. TypeScript simply adds features (like static types, interfaces, and other improvements) on top of JavaScript without taking away any of its flexibility.

Now, let's dive into a specific TypeScript error: **TS1085**.

## TS1085: Octal literals are not available when targeting ECMAScript 5 and higher. Use the syntax '{0}'.

### What Does TS1085 Mean?

The error **TS1085** occurs when you attempt to use octal literals in your TypeScript code while targeting ECMAScript 5 (or a higher version). An octal literal is a way to represent numbers in base 8, using digits from 0 to 7. For instance, `0o12` represents the decimal number `10`.

In ECMAScript 5 and onwards, JavaScript does not support octal literals in the old notation (e.g., `012` for decimal `10`). Instead, it introduced a new syntax that starts with `0o` followed by octal digits.

### Code Example Causing TS1085

Here’s an example that will trigger the TS1085 error:  

```
let num: number = 012; // This will cause TS1085
```

### Fixing the Error

To fix this error, update the octal number to use the correct syntax:  

```
let num: number = 0o12; // This is valid in ES5 and higher
```

## Important Things to Know

1. **Octal syntax`0o`**: Always start your octal number with `0o`.
2. **Compatibility**: Ensure that you are not using the old octal syntax (`012`).
3. **TypeScript Configuration**: Check your `tsconfig.json` file to see what ECMAScript version you are targeting.

### Further Clarification on TS1085

When you encounter **TS1085**:

- It means that you're using a syntax not allowed in ECMAScript 5 and above when dealing with octal numbers.
- Using the modern octal notation (`0o...`) resolves the error, making your code compliant with newer standards.

### Examples of Usage

```
// Correct usage of octal literals
let validOctal: number = 0o20; // This is valid, representing decimal 16
console.log(validOctal); // Outputs: 16

// Incorrect usage causing TS1085
let invalidOctal: number = 020; // This will cause TS1085
```

## FAQ's Section

- **What is an octal literal?**
    
    - An octal literal is a number represented in base 8.
    

- **Why must I use the `0o` prefix?**
    
    - This prefix defines the number as an octal in ECMAScript 5 and higher, avoiding ambiguity with decimal interpretations.
    

- **What happens if I ignore TS1085?**
    
    - Ignoring this error will result in run-time issues in JavaScript, as it won’t understand the invalid octal syntax.
    

In summary, always ensure to use the correct octal syntax in TypeScript to avoid **TS1085: Octal literals are not available when targeting ECMAScript 5 and higher. Use the syntax '{0}'**. Adhering to the standards keeps your code clean and ensures compatibility with the broader JavaScript ecosystem. Happy coding!

Go to Source

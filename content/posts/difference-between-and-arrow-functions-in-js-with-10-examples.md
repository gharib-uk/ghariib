---
title: "<div>Difference Between `( ) => { }` and `( ) => ( )` Arrow Functions in JS with 10 Examples</div>"
date: 2025-01-26
---

## Difference Between `( ) => { }` and `( ) => ( )` Arrow Functions in JavaScript with 10 real life examples

Click to Read Complete This blog with GitHub repository

Arrow functions are a popular feature in JavaScript introduced with ES6, simplifying the way functions are written. They come in two main syntaxes:

1. **Block Body**: `( ) => { }`
2. **Concise Body**: `( ) => ( )`

Understanding the difference between these two syntaxes is crucial as they differ in behavior, readability, and use cases. Let’s dive into the details.

## Key Differences Between `( ) => { }` and `( ) => ( )`

### 1\. **Block Body (`( ) => { }`)**

- This syntax uses curly braces `{}` to enclose the function body.
- Explicitly requires a `return` statement if you need to return a value.
- Suitable for multiline logic or when multiple operations are needed.

### 2\. **Concise Body (`( ) => ( )`)**

- This syntax directly returns an expression without curly braces.
- No need for an explicit `return` statement.
- Ideal for single-line computations or straightforward returns.

## Syntax and Examples

### Block Body Example:

```
const add = (a, b) => {
  return a + b; // Explicit return
};
console.log(add(2, 3)); // Output: 5
```

### Concise Body Example:

```
const add = (a, b) => a + b; // Implicit return
console.log(add(2, 3)); // Output: 5
```

## Key Differences with Examples

Here are 10 detailed examples illustrating the differences:

### 1\. **Return Behavior**

- **Block Body**:

```
  const greet = (name) => {
    return `Hello, ${name}!`;
  };
  console.log(greet("Alice")); // Output: Hello, Alice!
```

- **Concise Body**:

```
  const greet = (name) => `Hello, ${name}!`;
  console.log(greet("Alice")); // Output: Hello, Alice!
```

### 2\. **Multiline Logic**

- **Block Body**:

```
  const calculateArea = (length, width) => {
    const area = length * width;
    return area;
  };
  console.log(calculateArea(5, 10)); // Output: 50
```

- **Concise Body**:

```
  // Not suitable for multiline logic
  const calculateArea = (length, width) => length * width;
  console.log(calculateArea(5, 10)); // Output: 50
```

### 3\. **Object Return**

- **Block Body**:

```
  const getUser = () => {
    return { name: "Alice", age: 25 };
  };
  console.log(getUser()); // Output: { name: "Alice", age: 25 }
```

- **Concise Body**:

```
  const getUser = () => ({ name: "Alice", age: 25 });
  console.log(getUser()); // Output: { name: "Alice", age: 25 }
```

### 4\. **No Explicit Return**

- **Block Body**:

```
  const square = (x) => {
    x * x; // No return
  };
  console.log(square(4)); // Output: undefined
```

- **Concise Body**:

```
  const square = (x) => x * x;
  console.log(square(4)); // Output: 16
```

### 5\. **Side Effects**

- **Block Body**:

```
  const logMessage = (message) => {
    console.log(message);
  };
  logMessage("Hello!"); // Output: Hello!
```

- **Concise Body**:

```
  // Not suitable for side effects
```

### 6\. **Chaining Functions**

- **Concise Body**:

```
  const double = (x) => x * 2;
  const addTen = (x) => x + 10;
  console.log(addTen(double(5))); // Output: 20
```

- **Block Body**:

```
  const double = (x) => {
    return x * 2;
  };
  const addTen = (x) => {
    return x + 10;
  };
  console.log(addTen(double(5))); // Output: 20
```

### 7\. **Arrow Function as Callbacks**

- **Concise Body**:

```
  [1, 2, 3].map((x) => x * 2); // Output: [2, 4, 6]
```

- **Block Body**:

```
  [1, 2, 3].map((x) => {
    return x * 2;
  }); // Output: [2, 4, 6]
```

### 8\. **Usage with Ternary Operators**

- **Concise Body**:

```
  const isEven = (num) => (num % 2 === 0 ? "Even" : "Odd");
  console.log(isEven(3)); // Output: Odd
```

- **Block Body**:

```
  const isEven = (num) => {
    return num % 2 === 0 ? "Even" : "Odd";
  };
  console.log(isEven(3)); // Output: Odd
```

### 9\. **Returning Arrays**

- **Concise Body**:

```
  const getNumbers = () => [1, 2, 3];
  console.log(getNumbers()); // Output: [1, 2, 3]
```

- **Block Body**:

```
  const getNumbers = () => {
    return [1, 2, 3];
  };
  console.log(getNumbers()); // Output: [1, 2, 3]
```

### 10\. **React Functional Components**

- **Concise Body**:

```
  const Hello = () => <h1>Hello, World!</h1>;
```

- **Block Body**:

```
  const Hello = () => {
    return <h1>Hello, World!</h1>;
  };
```

## Use Cases

### Block Body `( ) => { }`

1. Suitable for complex logic.
2. Useful when explicit `return` improves readability.
3. Preferred for functions with side effects like `console.log`.

### Concise Body `( ) => ( )`

1. Ideal for one-liner functions.
2. Great for short computations and inline callbacks.
3. Enhances readability for simple expressions.

## Summary

| Feature | `( ) => { }` (Block Body) | `( ) => ( )` (Concise Body) |
| --- | --- | --- |
| **Syntax** | `{ }` with `return` | `()` without `return` |
| **Readability** | Better for complex logic | Cleaner for simple returns |
| **Return Statement** | Explicitly required | Implicit |
| **Multiline Logic** | Supported | Not suitable |
| **Side Effects** | Easily handled | Less commonly used |
| **Single-line Functions** | Verbose | Ideal |

Understanding these nuances allows you to choose the right arrow function syntax depending on your specific use case. Both syntaxes are powerful, and knowing when to use each one will make your JavaScript code more efficient and readable.

Go to Source

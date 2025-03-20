---
title: "<div>TS1071: '{0}' modifier cannot appear on an index signature</div>"
date: 2025-01-15
---

# TS1071: '{0}' modifier cannot appear on an index signature

TypeScript is a powerful superset of JavaScript that adds static typing to the language. This means that you can define types for your variables, function parameters, and return values, which helps catch errors during development rather than at runtime. Types are essentially a way to describe the shape of data, defining what kind of values are valid and what operations can be performed on them.

If you're eager to enhance your TypeScript knowledge or harness AI tools like gpteach to learn to code effectively, I invite you to subscribe or follow my blog for more insights!

## What is Typescript?

TypeScript builds on JavaScript by introducing optional static typing and other features that help developers write clearer and more maintainable code. One core aspect of TypeScript is its understanding of types. Types can represent primitive values like numbers, strings, or booleans, but they can also represent complex structures such as arrays, objects, and functions.

## What are Types?

Types in TypeScript define how data is structured. For example, you might have a type for a user object that has a name and an age:  

```
type User = {
    name: string;
    age: number;
};
```

This tells TypeScript that any variable of type `User` should have a `name` (which is a string) and an `age` (which is a number).

Now let's dive into the error `TS1071: '{0}' modifier cannot appear on an index signature.` This error occurs when you attempt to use a modifier (such as `readonly` or `public`) in an index signature of a TypeScript interface or type definition.

## TS1071: '{0}' modifier cannot appear on an index signature

### Understanding the Error

In TypeScript, an index signature allows you to define the shape of an object whose properties are not known at compile time. It’s typically used for objects that have dynamic keys. For example:  

```
interface StringMap {
    [key: string]: string;
}
```

This defines an object where any string key maps to a string value. However, you cannot use modifiers like `readonly` on the index signature itself:  

```
interface StringMap {
    readonly [key: string]: string; // This will cause TS1071: '{0}' modifier cannot appear on an index signature.
}
```

Instead, if you want to enforce properties as readonly, you need to wrap the object itself or use a different approach. Here’s how you might fix this error:

### How to Fix the Error

To avoid `TS1071: '{0}' modifier cannot appear on an index signature`, remove the modifier from the index signature and use it at the property level if necessary. For example:  

```
interface ReadonlyStringMap {
    readonly [key: string]: string; // This still causes TS1071.
}

type CorrectStringMap = {
    [key: string]: string;
};

const example: CorrectStringMap = {
    a: "Hello",
    b: "World"
};

// Assigning is allowed, since it's mutable
example.a = "Hi"; 

// To create readonly behavior, you might consider:
const readonlyExample: Readonly<Record<string, string>> = {
    a: "Hello",
    b: "World"
};

// This will cause errors if you try to modify readonlyExample.
// readonlyExample.a = "Hi"; // Error: Cannot assign to 'a' because it is a read-only property.
```

In this example, `readonlyExample` is defined such that its properties cannot be modified after initialization, avoiding the `TS1071: '{0}' modifier cannot appear on an index signature` issue.

### Important Things to Know

- **Index Signature**: A way to define types for objects with dynamic keys.
- **Modifications**: Modifiers like `readonly` or `public` cannot be applied directly to index signatures.
- **Readonly Behavior**: Use `Readonly<T>` or a similar approach to achieve immutability.
- **Error Handling**: Always look for the placement of modifiers in your TypeScript code to avoid this error.

By understanding the context of `TS1071: '{0}' modifier cannot appear on an index signature`, you can ensure that your TypeScript code remains clean and free of errors related to type definitions.

### FAQ

1. **What is an index signature?**
    
    - An index signature allows you to define an object type that can have multiple properties with the same value type, where the property names are not known ahead of time.
2. **Can I use `readonly` on an interface?**
    
    - Yes, but it cannot be applied directly in an index signature; consider other approaches like using `Readonly` utilities.

By keeping these guidelines in mind, you can navigate TypeScript's typing system and avoid the pitfalls associated with errors like `TS1071: '{0}' modifier cannot appear on an index signature`. Happy coding in TypeScript!

Go to Source

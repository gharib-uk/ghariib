---
title: "<div>TS1089: '{0}' modifier cannot appear on a constructor declaration</div>"
date: 2025-01-15
---

# TS1089: '{0}' modifier cannot appear on a constructor declaration

TypeScript is a powerful, statically typed superset of JavaScript that brings optional typing and many advanced features to the language. In TypeScript, types define the shape and behavior of data, allowing developers to build more predictable and maintainable code. Types can be primitive (like `number`, `string`, and `boolean`), complex (like objects and arrays), or user-defined (like `interfaces` and `enums`). If you want to deepen your understanding of TypeScript or explore how to use AI tools like gpteach to enhance your coding skills, consider subscribing to my blog!

### What is a Superset Language?

A superset language, like TypeScript, is built on top of another language (in this case, JavaScript). This means every valid JavaScript code is also valid TypeScript code. TypeScript adds additional features, such as static types and interfaces, to JavaScript. With these enhancements, developers can catch errors at compile time rather than runtime, leading to more robust applications.

Now, let's dive into the error represented by TS1089: '{0}' modifier cannot appear on a constructor declaration.

## Understanding TS1089: '{0}' modifier cannot appear on a constructor declaration

This error indicates that a modifier you are trying to use with a constructor (like `public`, `private`, or `protected`) is not allowed. These modifiers specify the access level of class members, but TypeScript has restrictions on their use concerning constructors.

### Example of the Error

```
class MyClass {
    private constructor() { // TS1089: 'private' modifier cannot appear on a constructor declaration.
        // constructor logic here
    }
}
```

In the example above, we are trying to declare a constructor as `private`. This will throw an error TS1089: '{0}' modifier cannot appear on a constructor declaration. To correct this, we need to change the design such that we do not define the constructor with an access modifier.

### How to Fix the Error

To fix the error, we need to either remove the access modifier or rethink the design of the class. For example:  

```
class MyClass {
    constructor() {
        // constructor logic here
    }
}
```

Here is an example where we need a class method that can only be accessed within the class, and keeping the constructor modifier is not applicable.

### Important to Know

- **Modifiers**: Access modifiers in TypeScript are used to control access to class members. You can use `public`, `private`, and `protected`, but they have specific rules when used in conjunction with constructors.
- **Static Members**: If a member is static, access modifiers work differently than with instance members. However, constructors themselves cannot use access modifiers in static context.
- **Abstract Classes**: Abstract classes can have protected or public constructors, but no modifiers can be added.

### Code Example Correcting TS1089

If you need a constructor to be private, but also want to use access modifiers elsewhere, consider using a factory pattern instead:  

```
class MyClass {
    private constructor() {
        // constructor logic here
    }

    static createInstance(): MyClass {
        return new MyClass(); // Valid since we're within a static method
    }
}
```

### FAQ

**Q: Why can't I use access modifiers on constructors?**  
A: The TypeScript language has specific rules about where and how access modifiers can be used, and constructors are not one of those places.

**Q: What if I need to control access to my constructor?**  
A: You can use a static factory method to create instances of your class, while keeping the constructor private.

**Q: What does the TS1089 error code indicate?**  
A: It signifies that you've attempted to use a modifier inappropriately on a constructor declaration, which is not allowed.

In summary, the TS1089: '{0}' modifier cannot appear on a constructor declaration reminds us of the restrictions around using access modifiers in TypeScript. By understanding how to structure your classes and adjust your constructors correctly, you can avoid this error and write cleaner, more effective TypeScript code.

Go to Source

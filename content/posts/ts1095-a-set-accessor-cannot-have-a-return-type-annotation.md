---
title: "<div>TS1095: A 'set' accessor cannot have a return type annotation</div>"
date: 2025-01-15
---

# TS1095: A 'set' accessor cannot have a return type annotation

TypeScript is a strongly typed programming language that builds on JavaScript by adding optional static types. This allows developers to catch errors early and write more maintainable code. Types are essentially contracts that define what kind of data can be stored and manipulated. They help ensure that your code operates as expected, reducing runtime errors related to incorrect data types. If you're interested in learning TypeScript or using AI tools like gpteach to enhance your coding skills, consider subscribing or following my blog!

To give you a better understanding of TypeScript, let’s briefly discuss what an interface is. An interface in TypeScript is a structure that defines the shape of an object, detailing which properties and methods it should have. It allows for better organization and type-checking in your code, facilitating collaboration and maintaining predictability in your software projects.

Now, let's delve into the issue of **TS1095: A 'set' accessor cannot have a return type annotation**.

### Understanding TS1095

This error arises when you attempt to annotate the return type of a `set` accessor in a class. In TypeScript, the `set` accessor is used to define how to set the value of a property. Unlike regular functions, `set` accessors do not return a value; they simply set the value of a property. Therefore, TypeScript does not allow a return type annotation for `set` accessors.

#### Example causing TS1095

Consider the following code snippet, which will lead to the TS1095 error:  

```
class Person {
    private name: string;

    constructor(name: string) {
        this.name = name;
    }

    set Name(value: string): string { // This line causes TS1095
        this.name = value;
    }
}
```

In the above code, the `set` accessor for the `Name` property incorrectly specifies a return type of `string`, which leads to the error **TS1095: A 'set' accessor cannot have a return type annotation**.

#### How to fix it

To resolve this error, simply remove the return type annotation from the `set` accessor. The corrected version of the `Person` class looks like this:  

```
class Person {
    private name: string;

    constructor(name: string) {
        this.name = name;
    }

    set Name(value: string) { // Now it's correct!
        this.name = value;
    }

    get Name(): string {
        return this.name;
    }
}
```

In this corrected example, the `set` accessor no longer has a return type annotation, and TypeScript will no longer throw the **TS1095: A 'set' accessor cannot have a return type annotation** error.

### Important things to know about TS1095

1. **Set accessors do not return values**: Understanding that `set` accessors are designed only to modify properties is crucial.
    
2. **Correct syntax**: Always ensure that `set` accessors are defined without return type annotations.
    
3. **Check for other accessor errors**: If you encounter other similar errors, make sure to look at the syntax and return type declarations of both `set` and `get` accessors.
    
4. **TypeScript version compatibility**: Make sure you are using an up-to-date version of TypeScript, as the rules evolve.
    
5. **Use proper tooling**: Leverage IDEs and linters that support TypeScript to catch these issues early during development.
    

In conclusion, when you encounter **TS1095: A 'set' accessor cannot have a return type annotation**, it’s essential to remember that `set` accessors are meant solely for mutating values and do not return a value. By omitting the return type annotation, you can quickly resolve this error and write cleaner, more effective TypeScript code. If you want to further enhance your TypeScript knowledge, don’t forget to check out gpteach for useful resources and tools!

Go to Source

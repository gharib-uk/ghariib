---
title: "Zero (0) vs Null vs Undefined: Understanding the Differences"
date: 2025-01-23
---

In programming, it’s common to come across 0, null, and undefined. These three values might seem similar at first glance, but they serve different purposes and behave differently in code. Let’s explore what each means and how they work, with simple examples.

## What is Zero (0)?

The value 0 represents a number, specifically the smallest non-negative number. It is a defined value and is treated as a number in JavaScript.

Example:  

```
let num = 0;
console.log(num); // Output: 0
console.log(typeof num); // Output: 'number'
```

Here, 0 is a valid number and can be used in calculations, comparisons, and other operations.

## What is Null?

null represents an intentionally empty value. It’s like saying, “This variable has no value right now, but I’ve intentionally left it empty.”

Example:  

```
let emptyValue = null;
console.log(emptyValue); // Output: null
console.log(typeof emptyValue); // Output: 'object' (this is a historical quirk in JavaScript)
```

Key point: You use null when you want to explicitly indicate that a variable has no value.

## What is Undefined?

undefined means that a variable has been declared but hasn’t been assigned a value yet. It’s like saying, “This variable exists, but no value has been given to it.”

Example:  

```
let notAssigned;
console.log(notAssigned); // Output: undefined
console.log(typeof notAssigned); // Output: 'undefined'
```

Key point: undefined is automatically assigned to variables that haven’t been given a value.

# Comparing null and undefined with == and ===

## Loose Equality ==

When you use ==, JavaScript performs type coercion, which means it tries to convert the values to the same type before comparing them.

Examples:  

```
console.log(null == undefined); // true (because they both represent "no value")
console.log(0 == undefined);   // false (0 is a number, undefined is not)
console.log(null == 0);         // false (null doesn’t equal 0)
```

## Strict Equality ===

When you use ===, JavaScript compares both the value and the type without any type coercion.

Examples:  

```
console.log(null === undefined); // false (different types: object vs undefined)
console.log(0 === undefined);   // false (number vs undefined)
console.log(null === 0);         // false (object vs number)
```

You can get further details about == vs === by referring to this article LINK

## Visualizing with Real-Life Example

Imagine you have three types of boxes:

1. `undefined` box:

- This is an empty box
- Nobody has even tried to put anything in it
- This “emptiness” happened accidentally/unintentionally

1. `null` box:

- This is also an empty box
- But someone intentionally emptied this box
- This is “intentionally emptied”

1. `0` box:

- This is not empty
- This has a real value — which is 0
- This is not “emptiness”, this is a number

That’s why:  

```
// Both are empty boxes so they are equal
console.log(null == undefined); // true
// 0 is a box with something in it, so not equal to empty boxes
console.log(0 == undefined); // false
console.log(null == 0); // false
```

# Conclusion

0 is a number and represents "nothing" in terms of quantity, but it’s still a defined value.  
null is an intentionally empty value, often used to indicate that something has no value at the moment.  
undefined means that a variable has been declared but hasn’t been assigned a value yet.  
Happy coding!

Go to Source

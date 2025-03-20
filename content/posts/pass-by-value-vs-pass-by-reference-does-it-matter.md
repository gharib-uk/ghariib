---
title: "Pass By Value vs. Pass By Reference, Does It Matter?"
date: 2025-01-20
---

_Pass by value_ versus _pass by reference_, what is it? I was asked this question on my very first tech job interview. I think my answer was that in pass by value a value is passed, while in pass by reference a reference is passed. Yes, this seems like a very basic and shallow answer. However, it is not entirely wrong, but a further explanation is needed.

**Pass by value:** The value of a variable is cloned and passed into a function as an argument to a parameter defined in the function. The function is executed and performs various operations with this cloned value. The value of the original variable is _UNCHANGED_. Almost all known programming languages (including JavaScript and Java), use this.

![Pass by value](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F3gm28rbbwwpki86bpj58.png)

**Pass by reference:** A reference is the variable pointing to a value in memory. Think of it as a pointer. It is passed as an argument into the parameter defined in the function. This reference is given an alias using the parameter (aliased reference). The function is executed and performs various operations using this parameter. This parameter now has access to the memory location of the value. Therefore, any operations using that parameter affects the value of the original variable. Therefore, the value of the original variable is _CHANGED_. Few programming languages use this. For example, C++ has the capability but even its default mode is pass-by-value.

![Pass by reference](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fa4l4he7x49706veeki97.png)

## Example of pass by value:

The coding example will use JavaScript which is strictly pass by value. This applies to both primitive and reference types (objects). This is the classic swap function.  

```
const firstPrimitive = 1; //primitive value to be passed to firstArgument
const secondPrimitive = 2; //primitive value to be passed to secondArgument

const firstObject = {name: "first", reason: "Test pass-by-value"}; //object to be passed to firstArgument
const secondObject = {name: "second", reason: "Test pass-by-value"}; //object to be passed to secondArgument

function swap(firstArgument, secondArgument){
    const placeholder = secondArgument;
    secondArgument = firstArgument;
    firstArgument = placeholder;

    console.log('Right now we are inside the function.');
    console.log('firstArgument = ', firstArgument, ', secondArgument = ', secondArgument, 'n');
}

console.log('Before function is executed.');
console.log('firstPrimitive = ', firstPrimitive, ', secondPrimitive =', secondPrimitive);
console.log('firstObject = ', firstObject, ', secondObject =', secondObject, 'n');

swap(firstPrimitive, secondPrimitive);
console.log('Right now we are outside the function.');
console.log('firstPrimitive = ', firstPrimitive, ', secondPrimitive =', secondPrimitive, 'n');

swap(firstObject, secondObject)
console.log('Right now we are outside the function.');
console.log('firstObject = ', firstObject, ', secondObject =', secondObject, 'n');
```

After we execute this code, we get the following.  

```
Before function is executed.
firstPrimitive =  1 , secondPrimitive = 2
firstObject =  { name: 'first', reason: 'Test pass-by-value' } , secondObject = { name: 'second', reason: 'Test pass-by-value' }

Right now we are inside the function.
firstArgument =  2 , secondArgument =  1

Right now we are outside the function.
firstPrimitive =  1 , secondPrimitive = 2

Right now we are inside the function.
firstArgument =  { name: 'second', reason: 'Test pass-by-value' } , secondArgument =  { name: 'first', reason: 'Test pass-by-value' }

Right now we are outside the function.
firstObject =  { name: 'first', reason: 'Test pass-by-value' } , secondObject = { name: 'second', reason: 'Test pass-by-value' }
```

Note that after executing the swap function that within the function the two values are exchanged. However, outside of the function the values are unchanged.

## Example of pass by reference:

This uses C++ which can be made to be pass-by-reference. This can be done using something called the address-of operator (&). This also uses a version of the swap function.  

```
#include <stdio.h>

void swap(int &i, int &j) {
  int temp = i;
  i = j;
  j = temp;
}

int main(void) {
  int a = 10;
  int b = 20;

  swap(a, b);
  printf("A is %d and B is %dn", a, b);
  return 0;
}
```

The output is:  

```
A is 20 and B is 10
```

The values of x and y here are changed since a and b are used as references.

**In summary:** _Pass by reference_ means a function receives the memory address of a variable (via reference), allowing it to directly modify the original variable, while _pass by value_ means a copy of the variable's value is passed without modifying the original variable.

Now the next and maybe even the most important question is does any of this matter? I ask this question because the overwhelming majority of programming languages use pass-by-value. It is not like we have a choice. How is this applicable? This is what I found on Google.

- Use pass by value when when you are only "using" the parameter for some computation, not changing it for the client program.
- Use pass by reference when you need a function to modify the original value of a variable passed as an argument, as it directly operates on the variable's memory location instead of creating a copy, making it more efficient, especially when dealing with large data structures like complex objects or arrays; essentially, it avoids the overhead of copying data and allows for direct changes to the original variable within the function. Essentially pass-by-reference can improve performance.

Feel free to comment. Thank you.

References:  
Is JavaScript Pass by Reference?  
Pass by reference (C++ only)

Go to Source

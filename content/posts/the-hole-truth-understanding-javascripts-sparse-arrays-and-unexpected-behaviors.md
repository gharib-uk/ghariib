---
title: "<div>The \"Hole\" Truth: Understanding JavaScript's Sparse Arrays and Unexpected Behaviors</div>"
date: 2025-01-12
---

I was recently brushing up on my Data Structures and Algorithms skills, specifically, sorting algorithms; and I found myself in an interesting situation.

How do I create a random array of length `n` of values to test my sorting algorithm? You might say that's easy, and it truly is. In your head you probably came up with something like this:  

```
function randomArray(n) {
    const arrToReturn = [];

    for (let i = 0; i < n; i++) {
        arrToReturn.push(Math.floor(Math.random() * 10));
    }
    return arrToReturn;
}

```

This definitely works as intended. But I was looking for something much simpler. Preferably a one-liner. Well, the first thought that came to mind was to use `new Array()`.  

```
const randomArray = (n) => new Array(n).map(() => Math.floor(Math.random() * 10));

console.log(randomArray(5))
```

What do you expect this to log?

Well, I was expecting this to log an array of random 5 values. If you weren't, then cheers! You know what I'm about to discuss :)

To my surprise, it's not the case. It logs this in Node 20:  
`[ <5 empty items> ]`

Hmmm ... Puzzling!

Interestingly, `console.log(randomArray(5).length)` logs `5`.

So, why does this happen? Well, let's talk about Sparse Arrays in Javascript!

### Sparse Arrays

Sparse arrays are arrays containing one or more empty slots. For instance:  

```
new Array(2)
// [<2 empty items>]

[1, , , 3]
// [1, <2 empty items>, 3]
```

So, how does this work?

Well, when you create an array in JS with `new Array(5)`, it creates an array with `5` uninitialized slots. This means that they do not contain anything; not `null`, not `undefined`.

_Okay, I get that! But can't you call `.map` on those "slots"?_

Well, when you call iterative methods such as `forEach`, `map`, `reduce`, and `filter`, etc... on a sparse array, these empty slots are skipped.

Let's take a look at our function `randomArray` again.  

```
const randomArray = (n) => new Array(n).map(() => Math.floor(Math.random() * 10));
```

We create an array of length `n` with `new Array(n)` which returns a sparse array `[<5 empty items>]`. Then we call `map` on that array. Since all the slots are empty, they all skipped. Hence, the result we got!

#### Why does calling `.length` on a sparse array return a value then?

This is due to the way `.length` array method is implemented in JS. To get the value of `length`, we take the largest index and just add 1. And since sparse arrays are indexed, we'll get the length value as expected. You can read more here.

#### Let's fix our buggy `randomArray` function above

Since sparse arrays contain empty or non-initialized slots, hence, not iterable, we can fix this by filling these slots with some values. We can achieve that by using the `.fill` array method:  

```
const randomArray = (n) => new Array(n).fill().map(() => Math.floor(Math.random() * 10));

console.log(randomArray(5))
```

And now, we're good to go!

Go to Source

---
title: "Removing an Annoying Compiler Optimization with a Hex-Rays Microcode Plugin"
date: Tue, 22 Jan 2019 22:39:50 +0000
draft: false
type: posts
---
# Removing an Annoying Compiler Optimization with a Hex-Rays Microcode Plugin

<br/>

<br/>
As part of my reverse engineering work, I wrote a small plugin to deal with an optimization that had been irritating me. The plugin isn't very sophisticated or interesting, but it is useful, and it's fully automatic, requiring no user interaction. You can find the code [here](https://github.com/RolfRolles/Miscellaneous/blob/master/ShiftAnd1Deoptimizer.cpp), and my [recent article on the Hex-Rays microcode API](http://www.msreverseengineering.com/blog/2018/9/19/hex-rays-microcode-api-vs-obfuscating-compiler) provides all of the necessary background.

In brief, testing individual bits within a quantity is a common pattern in C and C++ programming. For example, in the Windows API, after you have retrieved the attributes for a file, you might check to see if the file is a directory with an expression such as "(fileAttributes & FILE\_ATTRIBUTE\_DIRECTORY) != 0". The most obvious assembly-language implementation is simply with a "test" instruction against the constant:

![](https://images.squarespace-cdn.com/content/v1/53a64cc2e4b0c63fc41a3320/1548196137396-0CUAMROK0QA352QAEYRJ/ASM-unoptimized-impl.png?format=1000w)

However, in the binaries I've been analyzing lately, I frequently see a pattern such as the following, involving shifting the bit in question to the lowest position and performing an "and" or a "test":

![](https://images.squarespace-cdn.com/content/v1/53a64cc2e4b0c63fc41a3320/1548196202717-SX4R0F35108F7CDQ07U3/ASM-optimized-impl.png?format=1000w)

Honestly, I am not sure why this is an optimization. My first thought was that the shift/test sequence would be smaller, but they are actually the same size. The pattern manifests itself in the Hex-Rays decompilation as follows:

![](https://images.squarespace-cdn.com/content/v1/53a64cc2e4b0c63fc41a3320/1548196233043-94LKJWR3J0EF2AW91BVK/decomp-no-NOT.png?format=1000w)

Or, an even uglier variant involving a bitwise NOT:

![](https://images.squarespace-cdn.com/content/v1/53a64cc2e4b0c63fc41a3320/1548196243902-CVK90O6F32MP5U0NKMT5/decomp-NOT.png?format=1000w)

Because this optimization gets rid of the original constant value, we can't make use of Hex-Rays' nice features to convert constant values into enumeration elements. That means every time I see this pattern, I have to look up which enumeration element is being used, and leave the information in a comment. Obviously, it would be preferable to simply be able to use enumerations.

The problem was on my to-do list for long enough, and I finally got bored enough, that I did something about it. I wrote a small Hex-Rays Microcode API plugin to detect instances of the pattern and rewrite them using the proper constants, as in:

![](https://images.squarespace-cdn.com/content/v1/53a64cc2e4b0c63fc41a3320/1548196263261-NW7P8OEY4VENHN8H7FCL/decomp-altered-no-NOT.png?format=1000w)

And, for the bitwise NOT variant:

![](https://images.squarespace-cdn.com/content/v1/53a64cc2e4b0c63fc41a3320/1548196276849-3LFNQP1EOSGQM3LPKMIH/decomp-altered-NOT.png?format=1000w)

Much better. It took some extra work, but I also added the ability to change the constant to an enumeration. (The way Hex-Rays stores information about constants from the disassembly in the decompilation listing, and the rules under which you can convert them to enumeration elements, are somewhat complicated. The source code discusses them in more detail, along with some other information that might benefit Microcode plugin developers.) Here's an example:

![](https://images.squarespace-cdn.com/content/v1/53a64cc2e4b0c63fc41a3320/1548196292954-Q2OHZW6OJBNB80S7DHKN/decomp-enum.png?format=1000w)

Mission accomplished. In retrospect, it would have been easier and faster to write this as a CTREE plugin rather than a Microcode plugin, but I suppose all that matters is that it works. Maybe somebody else will find it useful.

#### [Source](https://www.msreverseengineering.com/blog/2019/1/22/removing-an-annoying-compiler-optimization-with-a-hex-rays-microcode-plugin)

<br/>
---

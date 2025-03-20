---
title: "Regular (Expression) Chess"
date: 2025-01-08
tags: 
  - "programming"
---

![](https://hackaday.com/wp-content/uploads/2025/01/chess.png?w=800)

\[Nicholas Carlini\] found some extra time on his hands over the holiday, so he decide to do something with “entirely no purpose.” The result: 84,688 regular expressions that can play chess using a 2-ply minmax strategy. No kidding. We think we can do some heavy-duty regular expressions, but this is a whole other level.

As you might expect, the code to play is extremely simple as it just runs the board through series of regular expressions that implement the game logic. Of course, that doesn’t count the thousands of strings containing the regular expressions.

How does this work? Luckily, \[Nicholas\] explains it in some detail. The trick isn’t making a chess engine. Instead, he creates a “branch-free, conditional-execution, single-instruction multiple-data CPU.” Once you have a CPU, of course it is easy to play chess. Well, relatively easy, anyway.

The computer’s stack and registers are all in a long string, perfect for evaluation by a regular expression. From there, the rest is pretty easy. Sure, you can’t have loops and conditionals can’t branch. You can, however, fork a thread into two parts. Pretty amazing.

Programming the machine must be pretty hard, right? Well, no. There’s also a sort-of language that looks a lot like Python that can compile code for the CPU. For example:

```
def fib():
    a = 1
    b = 2
   for _ in range(10):
        next = a + b
        a = b
        b = next
```

Then you “only” have to write the chess engine. It isn’t fast, but that really isn’t the point.

Of course, chess doesn’t have to be that hard. The “assembler” reminds us a bit of our universal cross assembler.

Go to Source

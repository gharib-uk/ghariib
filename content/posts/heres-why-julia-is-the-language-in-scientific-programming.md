---
title: "Here’s why Julia is THE language in scientific programming"
date: 2025-01-08
tags: 
  - "data"
  - "file"
  - "programming"
  - "tech"
  - "un"
---

One of the best things about working in quantum computing is being exposed to a whole new set of skills and tools. For those of us coming from the software engineering side, discovering the things our academic peers are using is an interesting experience. One of the best for me personally was coming across the Julia programming language, and learning why it has become the go-to language for serious scientific programming in some circles.

## What is the Julia programming language?

Julia is a relatively new programming language that has been gaining attention in the scientific computing communities. It was designed to combine the ease of use of Python with the performance of C. It also aims to be the go-to language for computationally intensive applications in fields like data science, scientific computing, machine learning, and over here in my world of quantum computing. Thankfully it’s not super difficult to get your head around and even easier to get started.

Julia was developed at MIT starting in 2009 and launched publicly in 2012 as an open-source project. It’s a dynamically-typed programming language built from the ground up for speed. Some of the key characteristics of Julia include:

- Just-in-time (JIT) compilation using the LLVM compiler framework, enabling performance on par with C/C++
- Optional type annotations, allowing it to be both dynamically and statically typed
- Metaprogramming capabilities for code generation and optimization
- Built-in package manager with an extensive ecosystem of libraries Familiar syntax, similar to Python and MATLAB Interoperability with C, Fortran, Python, R and more

<iframe width="710" height="399" src="https://www.youtube.com/embed/NF5InxVP3ZQ"></iframe>

Anyone talking about Julia usually points out how it is trying to solve the “two language problem”. This is where programmers use a high-level language like Python or R for prototyping and development, but have to rewrite performance-critical code in a lower-level language like C or Fortran. Which is something I feel in quantum computing where a lot of the machine level stuff is in C/C++. With Julia, the idea is that we can write high-level, expressive, and readable code without sacrificing performance. That’s why we see it popping up everywhere like:

- Data science and machine learning: Libraries like DataFrames.jl, MLJ.jl, and Flux.jl are building a powerful ecosystem for data work. The ability to write NumPy and pandas-like code with better performance is a big draw.
- Scientific computing: Packages like DifferentialEquations.jl and JuMP.jl are popular for modeling and simulation. Julia’s speed is a boon for compute-heavy applications.
- Parallel and distributed computing: Julia has strong support for parallel computing, both multi-threading and multi-process.
- Distributed computing frameworks like Dagger.jl make it easy to scale.

## Getting started with Julia

If you’re interested in trying Julia, the best place to start is, funnily enough, the official Julia website at julialang.org. There you can download the latest stable release for your platform and find extensive documentation and learning resources. Some great beginner resources include:

- The Julia Manual: Walks through the basics of the language with plenty of examples
- JuliaAcademy: Free interactive Julia courses from Julia Computing
- ThinkJulia: Free O’Reilly book teaching the basics of programming in Julia

https://www.youtube.com/watch?v=4igzy3bGVkQ

<iframe width="710" height="399" src="https://www.youtube.com/embed/4igzy3bGVkQ"></iframe>

Once you have Julia installed, you can launch the REPL (interactive command line) or use an IDE like Juno or VS Code with the Julia extension. Create a new file with a .jl extension, and you’re ready to start coding.

Here’s the obligatory “Hello, World!” in Julia:  

```
println("Hello, World!")
```

And here’s a quick example of Julia’s strengths — calculating the first 10 million prime numbers:  

```
julia
function is_prime(n)
 for i in 2:sqrt(n)
 if n % i == 0
 return false
 end
 end
 return true
end
primes = filter(is_prime, 1:10⁷)
println(length(primes))
```

This code runs in just a few seconds on a typical laptop, thanks to Julia’s ability to compile is\_prime into efficient native code.

## So thats Julia!

And there you go. A quick taste of the world of Julia. Hopefully that gets you on the path to checking out more, or just helps show you why we’re learning yet another programming language in a world absolutely chock full of them. Given how familiar it feels if you’re already using Python and MATLAB, this hopefully isn’t too much drama, and puts you in a good position to be able to make the most of the high-level scientific expression and low-level performance that makes it such a kick-ass tool for technical programming work. Good luck!

Go to Source

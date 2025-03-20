---
title: "Automation in Reverse Engineering C STLTemplate Code"
date: Tue, 21 Sep 2021 19:57:00 +0000
draft: false
type: posts
categories: 
- 
---
# Automation in Reverse Engineering C STLTemplate Code

<br/>

<br/>
There are three major elements to reverse engineering C++ code that uses STL container classes:

1.  Determining in the first place that an STL container is being used, and which category, i.e., `std::list<T>` vs. `std::vector<T>` vs. `std::set<T>`
2.  Determining the element type, i.e., `T` in the categories above
3.  Creating data types in your reverse engineering tool of choice, and applying those types to the decompilation or disassembly listing.

Though all of those elements are important, this entry focuses on the last one: creating instantiated STL data types, and more specifically, types that can be used in Hex-Rays. The main contribution of this entry is simply its underlying idea, as I have never seen it published anywhere else; the [code itself](https://github.com/RolfRolles/Miscellaneous/blob/master/STLTypes-ForDistribution.py) is simple enough, and can be adapted to any reverse engineering framework with a type system that supports user-defined structures.

I have spent the pandemic working on a new training class on C++ reverse engineering; the images and concepts in this blog entry are taken from the class material. The class goes into much more depth than this entry, such as through material on structure and type reconstruction, and by having individual sections on each of the common STL containers.

(If you are interested in the forthcoming C++ training class, it will be completed early next year, and available for in-person delivery when the world is more hospitable. If you would like to be notified when public in-person classes for the C++ course are available, please sign up on our [no-spam, very low-volume, course notification mailing list](https://www.msreverseengineering.com/training-classes). (Click the button that says "Provide your email to be notified of public course availability".) )

Overview and Motivation
=======================

At a language level, C++ templates are one of the most complex features of any mainstream programming language. Their introduction in the first place -- as opposed to a restricted, less-powerful version -- was arguably a bad mistake. They are vastly overcomplicated, and in earlier revisions, advanced usage was relegated to true C++ experts. Over time, their complexity has infested other elements of the language, such as forming the basis for the C++11 auto keyword. However, the basic, original ideas behind C++ templates were inconspicuous enough, and are easy to explain to neophytes. Moreover, reverse engineers do not need to understand the full complexity of C++ templates for day-to-day work.

Let's begin with a high-level overview of which problems in C software development that C++ templates endeavored to solve, and roughly how they solved them. Put simply, many features of C++ were designed to alleviate situations where common practice in C was to copy and paste existing code and tweak it slightly. In particular, templates alleviate issues with re-using code for different underlying data types.

C does offer one alternative to copy-and-paste in this regard -- the macro preprocessor -- though it is a poor, cumbersome, and limited solution. Let's walk through a small real-world example. Suppose we had code to shuffle the contents of a `char` array, and we wanted to re-use it to shuffle `int` arrays.

![](https://images.squarespace-cdn.com/content/v1/53a64cc2e4b0c63fc41a3320/1632252481851-6UAKUGDWIRG9IIIAN1VU/Example-Shuffle-C.png?format=1000w)

We could simply search and replace `char` for `int` this code, and also rename both functions (to, e.g., `swap_int` and `shuffle_int`). However, this is a poor solution; it would be nice if there was a language mechanism to do this automatically. Over time, C programmers came to use the macro preprocessor for problems like this. The following two-phase animated GIF shows the first step: wrapping the existing code in a macro declaration.

![](https://images.squarespace-cdn.com/content/v1/53a64cc2e4b0c63fc41a3320/1632252562110-QFVPUFXJZ3MHXSHG38PA/AnimMacros1.gif?format=1000w)

Next, we want to replace the elements that we wish to alter with macro parameters. In the following two-phase animation, we begin by replacing `char` with the macro parameter `TYPE`:

![](https://images.squarespace-cdn.com/content/v1/53a64cc2e4b0c63fc41a3320/1632252600437-X5R07RLGW923J1CDKTXE/AnimMacros2.gif?format=1000w)

Finally, we use the token-pasting operator `##` to rename the functions:

![](https://images.squarespace-cdn.com/content/v1/53a64cc2e4b0c63fc41a3320/1632252631789-3AN3WMIUED0XBE8NEOLU/AnimMacros3.gif?format=1000w)

Now, the process of converting the code to a macro is complete. We can do the same for the function declaration:

![](https://images.squarespace-cdn.com/content/v1/53a64cc2e4b0c63fc41a3320/1632252661956-8BMJ6BUIJ4UKILQ7KXWG/ShuffleHead.png?format=1000w)

And then put both the source and header macros into a single header file:

![](https://images.squarespace-cdn.com/content/v1/53a64cc2e4b0c63fc41a3320/1632252705741-L6DPYCT3ED1M7SMHSU83/ShuffleHeadBoth.png?format=1000w)

Now, we can use this header to create `shuffle` instantiations for any type we choose. For each desired type, simply include the header file from above and instatiate the macros:

![](https://images.squarespace-cdn.com/content/v1/53a64cc2e4b0c63fc41a3320/1632252740461-RLWXYKBTH7FPRZRC4DJI/ShuffleCharInt.png?format=1000w)

Then, the following three-phase animated GIF shows how the C preprocessor expands the macros for any given instantiation:

![](https://images.squarespace-cdn.com/content/v1/53a64cc2e4b0c63fc41a3320/1632252781911-S6J4NS4QX5YYLKI68W9V/AnimMacroExpand.gif?format=1000w)

A real-world example can be seen in the [uthash library](https://github.com/troydhanson/uthash/blob/master/src/utlist.h#L111).

The C++ template solution looks very similar to the C macro solution:

![](https://images.squarespace-cdn.com/content/v1/53a64cc2e4b0c63fc41a3320/1632252884068-7YX8JCM3BXCQGKN7QGWR/ShuffleTemplate.png?format=1000w)

The differences are that:

1.  `TYPE` becomes a `template typename` parameter.
2.  We don't need the line continuation characters (i.e., `\`).
3.  We don't need to rename the function.

The same concept really shines in the area of generic data structures. For example, here is the difference between a hypothetical `List` data structure as a C macro and as a C++ template. (Note that this definition of `List` is unrelated to the official C++ `std::list`, mentioned below.)

![](https://images.squarespace-cdn.com/content/v1/53a64cc2e4b0c63fc41a3320/1632252944399-USIQ47ZMQTC0ZV83PSVN/DataStructMacroTemplate.png?format=1000w)

C++ Template Containers
=======================

The C++ standard template library (STL) contains high-quality, generic implementations of common data structures. They are one of the best features of C++, and are used extensively by C++ programmers in real code. As hinted in the example above, they allow C++ programmers to immediately create instances with any types they choose.

One of the most popular STL data structures is known as `std::vector`. Vectors are like a better version of C arrays:

-   Like arrays, `std::vector` offers fast lookups using the same syntax (i.e. `arr[idx]` vs. `vec[idx]`).
-   Unlike arrays, `std::vector` knows the number of elements contained within the array (i.e. `vec.size()`).
-   Unlike arrays, `std::vector` can grow and shrink dynamically, without requiring the programmer to manually manage the memory (i.e. `vec.push_back("hi")` and `vec.pop_back()`).
-   Unlike arrays, `std::vector` offer bounds-checked indexing -- an imporant step for mitigating C's memory safety issues. I.e., `vec.at(500)` will throw an exception if `vec.size() < 500`, whereas `arr[500]` and `vec[500]` will not.
-   Programmers can obtain a pointer to the underlying array for compatibility with existing code that requires such pointers. Starting from C++11, this can be accomplished via `vec.data()`.
-   Unlike arrays, `std::vector` can own the elements it contains, such that deleting the `vector` will delete those elements. This simplifies resource management.

MSVC's implementation of `std::vector` is rather simple; when stripped to its core, it simply contains three pointers:

![](https://images.squarespace-cdn.com/content/v1/53a64cc2e4b0c63fc41a3320/1632252996209-YHX296Z69XFPUTNVOU2Q/VectorDecl.png?format=1000w)

(Before proceeding, it is important to note that the C++ standard specifies the class interfaces for these data structures, but not their internals. Different implementations of the STL -- such as the ones used by MSVC, and by GCC -- implement these container classes differently. Any concrete examples below come from MSVC; the data structures can and will differ for other STL implementations, but the idea in this blog entry can be adapted to any of them.)

These pointers locate the base of the allocation, the last used element, and the last allocated element, as in the following image:

![](https://images.squarespace-cdn.com/content/v1/53a64cc2e4b0c63fc41a3320/1632253033635-JYXR6ZZ47WFTRL79M8PN/VectorPointers.png?format=1000w)

Upon instantiating `std::vector<T>` with a concrete type for `T`, the C++ compiler will create a new `class` layout using the proper `T` pointers. For example, here we can see two different instantiations for `int` and `string`:

![](https://images.squarespace-cdn.com/content/v1/53a64cc2e4b0c63fc41a3320/1632253069092-0L4AW8EZ5FL1GFJ6TQQB/VectorIntString.png?format=1000w)

Reverse Engineering STL Template Containers
===========================================

When reverse engineering code that makes use of `std::vector<T>`, after discovering that a `std::vector<T>` is indeed in use, the next step is to discover which type was used for `T`. This comes down to standard structure recovery, and will not be covered in this blog entry. However, my upcoming C++ reverse engineering class will cover these steps in depth. Let's imagine that the reverse engineer discovers they are dealing with a `std::vector<int>`.

The next step is for the reverse engineer to recreate a data structure compatible with `std::vector<int>`, and to apply those types to the decompilation or disassembly listing. The reverse engineer could simply create a type like this:

![](https://images.squarespace-cdn.com/content/v1/53a64cc2e4b0c63fc41a3320/1632253153510-M3N1R9U6YP4KC6XHJYPD/ReVectorInt.png?format=1000w)

And then apply it to the decompilation. The code might look like this, before (left) and after (right):

![](https://images.squarespace-cdn.com/content/v1/53a64cc2e4b0c63fc41a3320/1632258761132-1EH8STWXC9W7H9XI998Q/HRVectorBeforeAfter.png?format=1000w)

Although creating a compatible `std::vector` facsimilie is not very difficult, creating these structures can be tedious work. For example, my [ComRAT IDB](https://www.msreverseengineering.com/blog/2020/8/31/an-exhaustively-analyzed-idb-for-comrat-v4) contains `std::vector<T>` instantiations for 16 different types `T`. Moreover, other STL containers are more complex; for example, `std::map` actually consists of 5 different, related structures.

The primary contribution of this blog entry is to simply inform the reader that they can create these structures programmatically. In IDA and Hex-Rays, this is as simple as using format strings and an API named `parse_decls`.

![](https://images.squarespace-cdn.com/content/v1/53a64cc2e4b0c63fc41a3320/1632253306346-8PMA9SQX56BK6HS9611Z/ScriptVectorExample.png?format=1000w)

One simply calls this function like `MakeVectorType("int")`, and one is greeted with a new type named `vector_int` in the local types window. For more complex scenarios where the type cannot be used in the `struct` name (for example, `int*` would lead to a `struct` with the invalid name `vector_int*`), the second, optional parameter can be used to create the name of the `struct`. I.e., `MakeVectorType("int*","pInt")` would create a `struct` named `vector_pInt` whose pointer types were `int **`.

And... that's more or less it! I have collected the common STL data types into the [linked script](https://github.com/RolfRolles/Miscellaneous/blob/master/STLTypes-ForDistribution.py), such that once the reverse engineer has figured out which types are being used, they can immediately create the structures with no effort on their part. Those types are:

-   `std::vector`
-   `std::list`
-   `std::map` and `std::set`
-   `std::deque`
-   `std::shared_ptr`

For one small added benefit, my script also knows the function prototypes for some common STL container class member functions. The reverse engineer can simply copy and paste these into the function prototype type declaration to immediately set a valid type for these member functions.

These same ideas can be adapted to other STL implementations, as well as custom template libraries. For example, Valve uses their own STL-esque template library; a few minutes worth of work can be used to produce a script comparable to the above.

Not so bad, is it? Reverse engineering C++ template code does not have to be difficult.

#### [Source](https://www.msreverseengineering.com/blog/2021/9/21/automation-in-reverse-engineering-c-template-code)

<br/>
---

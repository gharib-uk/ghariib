---
title: "Programming Ada: Atomics and Other Low-Level Details"
date: 2025-01-02
categories: 
  - "security"
---

![](https://hackaday.com/wp-content/uploads/2019/09/ProgrammingSystem.jpg?w=800)

Especially within the world of multi-threaded programming does atomic access become a crucial topic, as multiple execution contexts may seek to access the same memory locations at the same time. Yet the exact meaning of the word ‘atomic’ is also essential here, as there is in fact not just a single meaning of the word within the world of computer science. One type of atomic access refers merely to whether a single value can be written or read atomically (e.g. reading or writing a 32-bit integer on a 32-bit system versus a 16-bit system), whereas atomic operations are a whole other kettle of atomic fish.

Until quite recently very few programming languages offered direct support for the latter, whereas the former has been generally something that either Just Worked![™](https://s.w.org/images/core/emoji/15.0.3/72x72/2122.png) if you know the platform you are on, or could often be checked fairly trivially using the programming language’s platform support headers. For C and C++ atomic operations didn’t become supported by the language itself until C11 and C++11 respectively, previously requiring built-in functions provided by the toolchain (e.g. GCC intrinsics).

In the case of Ada there has been a reluctance among the language designers to add support for atomic operations to the language, with the (GNU) toolchain offering the same intrinsics as a fallback. With the Ada 2022 standard there is now direct support in the `System.Atomic_Operations` library, however.

## Defining Atomic Operations

As mentioned earlier, the basic act of reading or writing can be atomic based on the underlying architecture. For example, if you are reading a 32-bit value on a fully 32-bit system (i.e. 32-bit registers and data bus), then this should complete in a single operation. In this case the 32-bit value read cannot suddenly have 8 or 16-bits on the end that were written during said reading action. Ergo it’s guaranteed to be atomic on this particular hardware platform. For Ada you can use the `Atomic` pragma to enforce this type of access. E.g.:

```
A : Unsigned_32 with Atomic;
A := 0;

A := A + 1;
-- This generates:
804969f:    a1 04 95 05 08          mov    0x8059504,%eax
80496a4:    40                      inc    %eax
80496a5:    a3 04 95 05 08          mov    %eax,0x8059504
```

Now imagine performing a more complex operation, such as incrementing the value (a counter) even as another thread tries to use this value in a comparison. Although the first thread’s act of reading is atomic and writing the modified value back is atomic, this set of operations is not, resulting in a data race. There’s now a chance that the second thread will read the value before it is updated by the first, possibly causing the second thread to miss the update and requiring repeated polling. What if you could guarantee that this set of atomic operations was itself atomic?

The traditional way to accomplish this is through mutual exclusion mechanisms such as the common concept of mutexes. These do however come with a fair amount of overhead when contended due to the management of the (implementation-defined) mutex structures, the management of which uses the same atomic operations which we could directly use as well. As an example there are LOCK XADD (atomic fetch and add) and LOCK CMPXCHG (atomic compare and exchange) in the x86 ISA which a mutex implementation is likely to use, but which we’d like to use for a counter and comparison function in our own code too.

## Reasons To Avoid

Obviously, having two or more threads compete for the same resources is generally speaking not a great idea and could indicate a flaw in the application’s architecture, especially in how it may break modularity. Within Ada an advocated approach has been to use protected objects and barriers within entries, which provides language-level synchronization between tasks. A barrier is defined using the `when` keyword, followed by the condition that has to evaluate to `true` before execution can continue.

From a more low-level programming perspective as inspired by C code and kin, the use of directly shared resources makes more sense, and can be argued to have performance benefits. This contrasts with the philosophy behind Ada, which argues that neither safety nor ease of maintenance should ever be sacrificed in the name of performance. Even so, if one can prove that it is in fact safe and does not invite a maintenance nightmare, it could be worth supporting.

One might even argue that since people are going to use this feature anyway – with toolchain intrinsics if they have to – one may as well provide a standard library version. This is something that could be immensely helpful to newcomers as well, as evidenced by my recent attempt to port a lock-free ring buffer (LFRB) from C++ to Ada and running into the atomic operations details head-first.

## Fixing A Lock-Free Ring Buffer

In my original Ada port of the LFRB I had naively taken the variables that were adorned with the `std::atomic` STL feature and replaced that with the `with Atomic;` pragma, blissfully unaware of this being actually an improvement over the ‘everything is implementation dependent and/or undefined behavior’  elements in C++ (and C). Since I insisted on making it a straight port without a major redesign, it would seem that here I need to use this new Ada 2022 library.

Since the code uses both atomic operations on Boolean and Integer types we need the following two packages:

```
with System.Atomic_Operations.Integer_Arithmetic;
with System.Atomic_Operations.Exchange;
```

These generic packages of course also need to have a specific package defined for our use:

```
type Atomic_Integer is new Integer with Atomic;
package IAO is new System.Atomic_Operations.Integer_Arithmetic(Atomic_Integer);

type Atomic_Boolean is new Boolean with Atomic;
package BAO is new System.Atomic_Operations.Exchange(Atomic_Boolean);
```

These new types are defined as being capable of atomic read and write operations, which is a prerequisite for more complex atomic operations and thus featured in the package instantiation. Using these types is required to perform atomic operations on our target variables, which are declared as follows:

```
dataRequestPending : aliased Atomic_Boolean;
unread             : aliased Atomic_Integer;
```

The `aliased` keyword here means that the variable has to be in memory (i.e. have a memory address) and not just in a register, allowing it to be the target of an access (pointer) type.

When we want to perform an atomic operation on our variables, we use the package which we instantiated previously, e.g.:

```
IAO.Atomic_Subtract(unread, len);
IAO.Atomic_Add(free, len);
```

The first of which will subtract the value of `len` from `unread` followed by the second line which will add the same value to `free`.  We can see that we are now getting memory barriers in the generated assembly, e.g.:

```
lock add DWORD PTR [rsp+4], eax #,, _10
```

Which is the atomic addition operation for the x86 ISA, confirming that we are now indeed performing proper atomic operations. Similarly, for booleans we can perform atomic operations such as assigning a new value and returning the previous value:

```
while BAO.Atomic_Exchange(dataRequestPending, false) loop
    null;
end loop;
```

## Atomic Differences

I must express my gratitude to the commentators to the previous LFRB Ada article who pointed out these differences between `atomic` in C++ (and C) and Ada. Along with their feedback there are also tools such as the Godbolt Compiler Explorer site where it’s quite easy to drop in some C++ and Ada code for comparison between the generated assembly, even across a range of ISAs. Since I did not consult any of these previously consider this article my mea culpa for getting things so terribly wrong earlier.

Correspondingly, before passing off the above explanation as the absolute truth, I will preface it by saying that it is my best interpretation of The Correct Way![™](https://s.w.org/images/core/emoji/15.0.3/72x72/2122.png) in the absence of significant amounts of example code or discussions. Currently I’m adapting the LFRB code as described as above and will update the corresponding GitHub project once I feel relatively confident that I can dodge writing a second apology article.

For corrections and feedback, feel free to sound off in the comments.

Go to Source

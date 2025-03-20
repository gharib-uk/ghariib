---
title: "A Compiler Optimization involving Speculative Execution of Function Pointers"
date: Fri, 08 May 2020 01:22:33 +0000
draft: false
type: posts
---
# A Compiler Optimization involving Speculative Execution of Function Pointers

<br/>

<br/>
Today I discovered a neat optimization that I'd only heard about in graduate school, but had never seen in a real binary. Although the code below involves virtual functions in C++, the same technique would work for ordinary function pointers in C. A few other optimizations are referenced in the explanation below; all of them can be found in my presentation, "Compiler Optimizations for Reverse Engineers", which is [the course sample for one of my reverse engineering training classes](https://www.msreverseengineering.com/training).

The optimization has to do with increasing speculative execution performance for function pointers that nearly always target one particular destination. Note that this is in contrast to the compiler optimization known as "devirtualization", which is when a compiler can prove that a particular function pointer invocation **must always** target one particular location, and turns the indirect call into a direct one. The optimization described in this entry differs in that the function pointer might **nearly always** point to one location, but might **occasionally** point elsewhere. (These estimates of runtime behavior could be derived through profiling, for example.)

The following is a snippet of code that comes from Microsoft's Active Template Library (ATL). More specifically, the smart pointer known as CComPtr, held in atlcomcli.h. Here is the code; modest, and unassuming:

```
template <class T>
class CComPtr
{
    T* p;

    // Release the interface and set to NULL
    void Release() throw()
    {
        T* pTemp = p;
        if (pTemp)
        {
            p = NULL;
            pTemp->Release();
        }
    }
}
```

Being a template class library, the programmer is free to create their own classes based on CComPtr by specifying any class type for the template typename parameter T. Or rather, any class type that has a method named "Release" with signature "void Release()", as that function is invoked by the if-body in the code above. In this scenario, Release is a virtual function — that is to say, objects of type T contain a function pointer pointing to the implementation of a function called “void Release()”.

In the code below, T is specialized by (i.e., replaced with) a scary-looking ATL type name called ATL::CComObject<CSpThreadControl>. So in particular, here is the compiled version of CComPtr<ATL::CComObject<CSpThreadControl>>::Release, whose generic C++ code was shown above.

The first four lines aren't interesting:

```
.text:18005121B   mov   rcx, [rcx]         ; T* pTemp = p;
.text:18005121E   test  rcx, rcx           ; if (pTemp) {
.text:180051221   jz    short return       ; [if not taken, return]
.text:180051223   and   qword ptr [rax], 0 ;     p = NULL;
```

The final line, the call to pTemp->Release, has a longer compiled body than one might expect. A line-by-line explanation follows below.

```
.text:180051227   lea   rdx, offset ATL::CComObject<CSpThreadControl>::Release
.text:18005122E   mov   rax, [rcx]
.text:180051231   mov   rax, [rax+10h] ; rax now contains the pTemp->Release pointer
.text:180051235   cmp   rax, rdx       ; did rax match the fixed location in rdx above?
.text:180051238   jnz   short no_match
.text:18005123A   call  ATL::CComObject<CSpThreadControl>::Release
.text:18005123F
.text:18005123F return:
.text:18005123F   add   rsp, 28h
.text:180051243   retn
.text:180051244
.text:180051244 no_match:
.text:180051244   add   rsp, 28h
.text:180051248   jmp   rax
```

To explain the code above:

-   Line #-27: move the offset some specific function into rdx.
    
-   Line #-2E through -31: rax now contains the pTemp->Release function pointer.
    
-   Line #-35 through -38: Compare rax and rdx. If equal, don't take the jump.
    
-   Line #-3A through -43: Invoke ATL::CComObject<CSpThreadControl>::Release(pTemp). **The important thing to notice is that the function being called is the same one whose offset was moved into rdx on the first line.**
    
-   Line #-44 through -48: If rax had not been equal to rdx, call the function pointer in rax. (More precisely, jump there after destroying the stack frame, as the result of an optimization known as tail call elimination.)
    

Reflecting on the code above, it might seem weird that it:

1.  Compares a function pointer to the address of a known function, let’s call it X.
    
2.  Calls the known function X directly if it matches.
    
3.  Calls the function pointer if it doesn't.
    

In other words, If the function pointer does match X — regardless of the comparison and direct call — then X would be the function invoked anyway by part #3. The indirect call in part #3 is sufficient; parts #1 and #2 would seem to be superfluous. Moreover, we can clearly see that this behavior was not present in the original C++ code: it was introduced by the compiler. Therefore, why would Microsoft Visual C++ — an otherwise heavily-optimizing compiler — insert the comparison and direct call?

What's happening here is that indirect calls are worse for the processor's speculative execution, since the processor needs to know where the call is going before it can start executing it. Direct calls have better performance, because their destinations are fixed and known. Technically, the virtual function call to Release() could go anywhere -- it is a function pointer, after all. However, because the template is specialized for the type T = ATL::CComObject<CSpThreadControl>, it's reasonable to expect that the virtual function will actually point at &ATL::CComObject<CSpThreadControl>::Release. In fact, profiling might determine that this is the destination on all, or nearly all, of the profiling data. So by performing an explicit comparison, we allow the processor's speculative execution to begin executing the probable destination before the real, runtime destination is known. As a result, if the virtual function usually points where we expect, we’ll get better speculative execution success, and hence better performance.

There's another neat aspect to the code above, also involving speculative execution. Notice that the jump after the comparison on lines #-35 through -38 is in the forward direction. The Pentium 4+ static branch prediction algorithm assumes that jumps that go in the forward direction won't be taken. Therefore, the compiler arranges the indirect function call to be at the end of a forward branch, which encourages the processor to speculatively execute the direct call target on the not-taken side instead of down the path with the indirect call.

#### [Source](https://www.msreverseengineering.com/blog/2020/5/7/a-compiler-optimization-involving-speculative-execution-of-function-pointers)

<br/>
---

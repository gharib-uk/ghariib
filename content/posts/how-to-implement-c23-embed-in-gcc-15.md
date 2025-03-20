---
title: "How to implement C23 #embed in GCC 15"
date: 2025-02-01
tags: 
  - "embed"
  - "endif"
  - "include"
---

GCC 15 is expected to be released in April or May 2025. To speed up compilation, consider using the `#embed` directive for programs which need to include larger binary data. Even programs using large array initializers may encounter nice compilation time speedups when using the new GCC version. This article discusses the inclusion of binary data in C as well as `#embed` implementation in GCC 15.

## 4 methods to add binary data

Various C/C++ programs need to include binary data in their binaries or shared libraries. There have been ways to achieve this with a few drawbacks.

### Method #1

One method is to use the `xxd -i` program (or other similar or hand written variants), such as the following:

```plaintext
xxd -i < sound.ogg > sound.h
```

This results in hex 0x00 to 0xff literals separated by spaces, as follows:

```plaintext
xxd -i sound.ogg > sound.h
```

This will add the following before and after it:

```c
unsigned char sound_ogg[] = {
```

```c
};
unsigned int sound_ogg_len = NNN;
```

### Method #2

Another method to add binary data is the GNU assembler `.incbin` directive, which you can use from inline `asm` as follows:

```c
asm (".section .rodata; .type sound_ogg, @object; "
     "sound_ogg: .incbin "sound.ogg"; "
     ".size sound_ogg, .-sound_ogg; .previous");
```

### Method #3

The third method of adding binary data is to use the GNU `objcopy` program, as follows:

```plaintext
objcopy --add-section sound_ogg=sound.ogg sound.o sound2.o
```

This adds the binary into `sound_ogg` section, and a linker script can arrange right placement of that section and add symbols for its start and end:

```plaintext
objcopy -I binary sound.ogg -O elf64-x86-64 sound3.o
```

This will create a new `sound3.o` object where `sound.ogg` is placed into `.data` section with `_binary_sound_ogg_start` and `_binary_sound_ogg_end` symbols showing start and end and the size `_binary_sound_ogg_size`. 

Alternatively, you can us the linker `-b` option as follows:

```plaintext
ld -r -o sound4.o -b binary sound.ogg
```

The main advantage of going through `xxd -i` or similar approaches is more portability. You won't need to copy the binary files around when using distributed compilation (`distcc`, etc.) for large amounts of data which may result in slow compilation and a lot of memory used during compilation.

Other approaches are less portable, and the size is known only at link time, not at compile time.

### Method #4: embed preprocessing directive

The recently published revision of the C standard ISO/IEC 9899:2024 (informally C23) comes with another portable variant, the `#embed` preprocessing directive. 

GCC 15 has implemented support for the `#embed` preprocessing directive as part of the rest of the C23 support, where `-std=gnu23` becomes the default C version and also as an extension for older C versions. It comes with pedantic warnings or errors if requested. Also, `#embed` has been proposed for standardization for C++ even before a proposal for C, is implemented in GCC 15 as well and again with pedantic warnings or errors if requested.

You can use this directive as follows:

```c
unsigned char sound_ogg[] = {
#embed "sound.ogg"
};
```

In this case, `#embed` works as if it was replaced by a sequence of 0 to 255 constants separated by commas as follows:

```c
79,103,103,83,0,2,0,0,0,0,0,0,0,0,116,49
```

Additionally, the compiler can try to compile it more efficiently, requiring less compile time and using less memory.

The directive can accept various standard parameters, or by extension, vendor-specific parameters after the filename. You can test using the `__has_embed` preprocessing operator whether such a directive would work, whether the file exists, and whether it is empty or not:

```c
#ifdef __has_embed
#if __has_embed ("sound.ogg" limit(42) prefix(1,) suffix(,0) if_empty(0)) 
== __STDC_EMBED_FOUND__
unsigned char sound_ogg[] = {
#embed "sound.ogg" limit(42) prefix(1,) suffix(,0) if_empty(0)
};
#endif
#endif
```

This will define the `sound_ogg` variable only if `sound.ogg` exists and is not empty and if the `#embed` directive will handle those parameters. All of them are standard, so they need to be supported by any C23 conforming implementation.

- It will include at most 42 bytes from it.
    
- Add `1,` tokens before it and `,0` tokens after it if not empty.
    
- If it is not empty, the `if_empty(0)` clause will not do anything since the `__has_embed` would be `__STDC_EMBED_EMPTY__` if it was empty.
    
- But when not guarded with `__has_embed`, that would expand to the `0` token if the file is empty.
    

You could test vendor extensions using `__has_embed` similarly as well.

## Performance

Now I will demonstrate how it performs when including larger binary data and how I implemented it.

All testing has been done with 2024-11-22 GCC 15 snapshot configured with `--enable-checking=release` on x86\_64-linux. As a test case, I chose two binaries from that GCC build: 9.2MiB large `collect2` and the first 100000000 bytes from `cc1plus` as follows:

```c
dd if=cc1plus of=/tmp/cc1plus bs=100000000 count=1
xxd -i < /tmp/collect2 > /tmp/collect2.h
xxd -i < /tmp/cc1plus > /tmp/cc1plus.h
```

This results in 57MiB and 589MiB headers all tested with these 3 compilers. 

1. gcc-b243 is that GCC snapshot configured and bootstrapped against binutils 2.43. 
    
2. gcc-b237 is the same GCC configured and bootstrapped against binutils 2.37. 
    
3. gcc-noopt-b237 is the same snapshot with the C array optimization and `CPP_EMBED` support artificially disabled with a small patch. So this one in behavior matches roughly GCC 14 behavior, except that it has a dumb `#embed` support.
    

The 4 test cases are as follows:

1.

```c
unsigned char c1[] = {
#include "/tmp/collect2.h"
};
```

2.

```c
unsigned char c2[] = {
#include "/tmp/cc1plus.h"
};
```

3.

```c
unsigned char c3[] = {
#embed "/tmp/collect2"
};
```

4.

```c
unsigned char c4[] = {
#embed "/tmp/cc1plus"
};
```

All of these cases were compiled and assembled with `gcc -c c_N_.c`.

The times in the following table are from user time printed by `time gcc -c c_N_.c` which includes both time to compile and assemble it. The maximum resident memory size is from `gcc -c c_N_.c -wrapper,/usr/bin/time,-f,%M`.

|   compiler   |   c1time   |   c1mem   |   c2time   |   c2mem   |   c3time   |   c3mem   |   c4time   |   c4mem   |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
|   gcc-noopt-b237   |   14.4s   |   1.3GiB   |   144s   |   10GiB   |   12.5s   |   1.3GiB   |   139s   |   13GiB   |
|   gcc-b237   |   3.6s   |   623MiB   |   26.6s   |   3.2GiB   |   0.5s   |   30MiB   |   5.6s   |   117MiB   |
|   gcc-b243   |   3.3s   |   623MiB   |   3.3s   |   3.2GiB   |   0.15s   |   31MiB   |   1.26s   |   117MiB   |

I've compiled a similar test case to c4 with 6 copies of full cc1plus concatenated (i.e., 2.2GiB of data) with gcc-b243 compiled in 28.3s with 2.2GiB memory needed. That is something that wouldn't work when using `xxd -i` generated header in gcc-noopt-b237 or GCC 14 due to GCC internal limitations of less than 231 elements in initializers. If it was below 2GiB, it would be too slow.

As you can see from the numbers, using `#embed` will be very fast for larger data and allow you to include data in sizes that older versions of GCC wouldn't be able to compile or would take too long and too many resources.

## #embed optimizations in GCC

I started with a dumb implementation of `#embed` support. This parses everything according to the specification. Then it reads the required files and caches normal files. Later, it replaces the directive with a sequence of `CPP_NUMBER` tokens separated by `CPP_COMMA`, which is roughly the state previously shown in the gcc-noopt-b237 numbers. Once that was done, I implemented more optimizations.

**Note:** There is actually an implementation divergence from strict reading of the C23 standard in all `#embed` implementations, regarding macro expansion in the optional parameters. All the current implementations macro expand everything after the filename unconditionally. While C23 would need to try to parse the whole directive tentatively with macro expansion disabled to find out if it matches the required grammar. Then based on that, C23 would decide whether to macro expand everything or only arguments of selected parameters, waiting for ISO WG14 to change this in a defect report or not.

The tokens from the `#embed` preprocessing directive can appear anywhere in the C source file. The tokens from `prefix`, `suffix`, or `if_empty` parameters are copied over or copied with macro expansion to the sequence of preprocessing tokens. As far as the tokens from the actual file, either there are no tokens (i.e., the file is empty), or it could be a single integer constant (e.g., with `limit(1)` or if the file has size of 1 byte), or multiple tokens separated by commas. Now, the no tokens or one integer constant case can appear in too many places in the C23 grammar. It would be a maintenance nightmare to use anything different from what the FE is used for (i.e., no tokens or one `CPP_NUMBER` token).

Furthermore, nothing in C23 prevents surrounding `#embed` with tokens, such as `23 +` tokens before it and `* 42` tokens after it or there could be a `(float)` before it. It should expand as if it is a sequence of comma separated constant literals. Therefore it is useful for the first and last constant in the sequence coming from `#embed` to also be represented normally, otherwise parsing would have to make the new token type a special case in many places. The final choice was to only optimize `#embed` with 64 bytes or more in it and emitting a token sequence for it, where `CPP_EMBED` is a newly introduced preprocessor token kind which has a pointer at the start of the data in the preprocessor's buffer and its length as follows:

```plaintext
CPP_NUMBER CPP_COMMA CPP_EMBED CPP_COMMA CPP_NUMBER
```

In order to reuse existing GCC data structures, I chose to limit the length of one `CPP_EMBED` to 2GiB - 1. If `#embed` is larger, there can be multiple `CPP_EMBED` tokens separated by `CPP_COMMA` in between. In any case, the first and last `CPP_NUMBER` are the first and last byte's values, yet the preprocessor buffer actually contains even those bytes.

This way, the front-ends which parse the preprocessor tokens can handle the new `CPP_EMBED` token in places, wherein the language grammar, a sequence of integer literals separated by commas is valid. In the C front-end, that can be primarily in braced initializers. This is the main expected use and the only one that is currently optimized. It can also appear in comma expressions such as `a = (1,2,3,4,5,6,7,8,9);` which is equivalent to just `a = 9;`. Users should surely get at least one `-Wunused-value` warning. 

However I'm not sure if they need millions of them for one `#embed` expression in there, so the parsing just parses it as an integer constant equal to the last byte in `CPP_EMBED`. Then it can appear in a helper function used for parsing (e.g., arguments of call expression, arguments of attributes or some OpenMP and OpenACC comma separated lists). Then the code turns it into a long list of integer constants. Similarly, another routine used for arguments of call-like expressions with special keywords - mainly for `__builtin_shufflevector`, other such built-ins usually have three arguments at most.

On the compiler side, we already had a tree (node of the GCC high level intermediate language) for a kind of binary blob data, `STRING_CST`. This is normally used for string literals but some optimizations could turn the whole initializer of some `char` array into a string literal, merging all the values from the initializer. The major disadvantage of `STRING_CST` for large initializers using `#embed` is that it owns the data. When we think in gigabytes of data, we really don't want to copy the data around. Ideally it stays in the preprocessor buffer and is referenced from it (with pointer into it and length). 

Also, the way C/C++ braced initializers work, we need a cheap way to peel off individual integer constants from `CPP_EMBED` (or its compiler counterpart) or split it into two or more parts. That is because in C/C++, you can omit braces around some initializers so different sets of the sequence can be used as initializers for different subobjects. Otherwise, with C99 designated initializers, you can overwrite some initializer in the middle of long array or you can adjust those later with C++ constant expression evaluation. 

The following test case shows this, as follows:

```c
struct S { int a, b; double c; unsigned char d[10000];
           struct T { int a; char b; } e; char f[20000]; int g; } s = {
#embed "data.bin" limit(30006) suffix(, .f[10000] = 42)
};
```

We get from the preprocessor one `CPP_NUMBER` (that is what is assigned to `s.a` member), then after `CPP_COMMA`, one `CPP_EMBED` which stands for 30004 numbers with commas in between. We need one number to initialize the `b` member with `int` type, one to initialize `c` member with `double` type, so those peel off one number, decrease `CPP_EMBED` length, and increase pointer into the buffer. 

Then we have an array of 10000 `unsigned char`s. This is a good place to use the new `RAW_DATA_CST` tree. But we should only use 10000 of the bytes in it, then two other peeled off numbers, and the next 20000 this time with `char`. So on targets with signed `char`, we need to warn if any of the bytes is higher than `SCHAR_MAX`, and the implicit conversion actually makes it negative (for C++ this might be in some cases a narrowing error). 

Finally, the separate `CPP_NUMBER` is parsed into `s.g` initialization. But then there is a designated initializer which overwrites the value in the middle of the 20000 elements long initializer. So we need to be able to split it cheaply into `RAW_DATA_CST`, covering the first 10000 values, then `INTEGER_CST` for the 42 stored in there, and finally another `RAW_DATA_CST` for the remaining 9999 values.

The new `RAW_DATA_CST` tree has three arguments, an owner of the data (which can be another `RAW_DATA_CST` with `NULL` owner to represent data owned by preprocessor in its buffer or a `STRING_CST`), pointer to the first byte and length. The range of bytes covered by the pointer and length needs to be within the data of the owner. 

There are multiple reasons for the owner member. One of which is the precompiled header support, where the preprocessor buffers aren't saved into the `*.gch` file and so we actually need to create a `STRING_CST` which will have the actual bytes. Another reason is LTO (link time optimization), where we also don't have the preprocessor buffers and don't want to save huge amounts of data for every raw data view which accesses the same data. So for LTO, we actually save the `RAW_DATA_CST` with `NULL` owner into something that is read back as a `STRING_CST`.

Finally, the compiler attempts to optimize the common case and get rid of the first and last number when `#embed` is the sole content of a braced initializer with the owner containing the first and last byte in there. The compiler can extend the `RAW_DATA_CST` view if the initializer contains the expected constants.

The `RAW_DATA_CST` also has a special purpose inside of the `CONSTRUCTOR` trees, which GCC uses for aggregate initializers in the GENERIC IL. The `CONSTRUCTOR` tree contains a vector of elements, where each element has a pair of value and index. While the index can be `NULL` with the meaning one higher than the previous element's index, the C and C++ front-ends fill those in. 

For arrays, the indices are integer constants representing which element it is. That is important for C because of the designated initializers. An array can be initialized with them in any order and there can be gaps. For C++, there are no designators for array initializers in the standard, and GCC doesn't allow out of order initialization or skipping of elements. But still through constant expression evaluation one can actually initialize array elements in any order. 

This is also a reason for the large memory consumption when not using `#embed`. GCC shares integer constant trees for the same type. So unsigned `char` array initializers can have a maximum 256 different trees. But 1000000 bytes long array fully initialized will also need 1000000 different integer constants (from 0 to 999999) for the indexes, and while those can be shared between different arrays, it still requires a significant amount of memory. The `RAW_DATA_CST` element inside of `CONSTRUCTOR`, which has an index specified, acts as if it contains the length of the data view numbers in it. So we need only one index: the index of the first byte in the `RAW_DATA_CST`. 

## Large binary data in assembly

During testing, I discovered that it took significant time to emit the large array initializers into the assembler and for the assembler to assemble. GCC usually emitted binary data using `.string` and `.ascii` directives if expected to be a string literal, or using `.byte` etc. directives for the individual members if the initializers haven't been merged. 

The `.string` and `.ascii` directives are nicely compact for printable bytes, one byte per character. But for control characters and characters above `SCHAR_MAX`, GCC was emitting either two bytes (e.g., `n` or `f`) or 4 bytes backslash and 3 octal digits, which is a lot. Plus `‘’` characters were handled by ending the `.string` directive and starting another one on another line.

GNU binutils 2.43 added a `.base64` directive where you can emit base64 encoded data with 4/3 bytes per character. Encoding and decoding it is very fast, so the compiler can choose between `.string`/`.ascii` or `.base64` and pick what is more compact. These numbers indicate that it brings noticeable speedups.

## Optimization for array initializers without #embed

When I implemented this optimization, I used a similar approach to slightly optimize the cases where source code contains large sequences of numbers from 0 to 255 separated by commas inside of braced initializers. The parser can detect that by using some look ahead, and if it sees a sequence of 64 or more constants separated by commas, it can turn it into a `RAW_DATA_CST` as well. 

One doesn't avoid the time to preprocess those or memory created for those preprocessor tokens. In GCC, the C front-end right parses tokens as needed, while C++ front-end parses all of them at once due to a much larger amount of tentative parsing. So the optimization saves less memory for C and more for C++. In any case, indices don't need to be created for each byte and compiler optimizations can also treat it as a large blob of binary data.

## Textual preprocessing of #embed

Users preprocess separately from compilation, whether manually by preprocessing with `gcc -E` or `gcc -E -fdirectives-only` and then compiling the preprocessed source later, or by using `-save-temps`, or by using `ccache`. A dumb `#embed` implementation would just preprocess `#embed` into a sequence, like the following:

```c
79,103,103,83,0,2,0,0,0,0,0,0,0,0,116,49
```

But that can be up to 4 bytes per character and expensive to parse back.

It is desirable to include the data in the preprocessed source, otherwise `ccache` won't work properly. If the data changes, it wouldn't know it needs to regenerate, and `distcc` might not work at all. The data might be on one machine and not on others. 

Because `#embed` with vendor extension parameters is pedantically valid if the implementation supports it (so no `-pedantic-errors` for it), GCC chose to use an `#embed` directive extension to emit the data in compact form as follows:

```c
    78,
# 1 "test.c"
#embed "." __gnu__::__base64__( 
"b24gZXJhbSBuw6lzY2l1cywgQnJ1dGUsIGN1bSwgcXXDpiBzdW1taXMgaW5nw6luaWlzIGV4cXVp" 
"c2l0w6FxdWUgZG9jdHLDrW5hIHBoaWzDs3NvcGhpIEdyw6ZjbyBzZXJtw7NuZSB0cmFjdGF2w61z" 
"c2VudCw=")
# 1 "test.c"
,10
```

The first and last values from `#embed` are 78 and 10. The rest is base64 encoded in the new `gnu::base64` parameter, which can be used only with `"."` filename.

GCC has also added the `gnu::offset` parameter so you can seek into a file, bypassing a known header at the start of it, similarly how the GNU assembler's `.incbin` directive has the skip optional parameter.

## Summary

Programs that need to include larger binary data might consider using the `#embed` directive at least conditionally to speed up compilation. But even if programs use large array initializers, they may experience faster compilation time when using the GCC version 15 or newer.

The post How to implement C23 #embed in GCC 15 appeared first on Red Hat Developer.  
  

Go to Source

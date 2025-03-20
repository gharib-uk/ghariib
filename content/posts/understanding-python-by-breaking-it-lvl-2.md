---
title: "Understanding Python by breaking it - Lvl 2"
date: 2025-01-19
categories: 
  - "cybersecurity"
  - "cybersecurity-awareness"
  - "security"
  - "security-awareness"
  - "vulnerabilities"
tags: 
  - "ob_refcnt"
---

After my first article I kept playing with `ctypes` and tried to see what fun things that could be possible with little modifications to the Python's objects at runtime. The goal of this article is to explain how to **get reliable native code execution as a simple Python objet**. By reliable I mean: that, if our payload respect certain rules, we can call it multiple times from the same Python code.

If you have not read my previous article, here is what you need to know:

- the `ctypes` module allows you to manipulate C structure from Python,
- we can map a `ctypes` structure on a given address using `struct.from_address`,
- in CPython: `id(obj)` returns the address of obj.

## Using `ctypes.Structure`

Last time, I only used `id(obj)` to access the internal variables of an object. This time we need to do more complex things, so I am going to use the `ctypes.Structure` type for more readability.  
Here is an example:

```
import ctypes
>>> class PyObj(ctypes.Structure):
...     _fields_ = [("ob_refcnt", ctypes.c_size_t),
...                 ("ob_type", ctypes.c_void_p)] # Must be casted

# Inheritance of ctypes.Structure: PyVarObj will also have the fields of PyObj
>>> class PyVarObj(PyObj):
...     _fields_ = [("ob_size", ctypes.c_size_t)]

>>> l = [1,2,3,4] 
>>> lm = PyVarObj.from_address(id(l))
>>> lm.ob_refcnt
1L
>>> lm.ob_size
4L
>>> lm.ob_size = 2
>>> l
[1,2]
```

Now we have a nice way to access to C-only data with readable and explicite names!

So, let's get to work !

## Dummy code execution

### Instruction Pointer Control

Before having a nice and reliable native code execution, the first step is having code execution at all. The most obvious type we want to check is _built-in function_, this is a Python type that represents native code.

Let's have a look at the structure of a _built-in function_:

```
/** My own comments begin by '**' **/
/** From: Include/methodobject.h **/

typedef PyObject *(*PyCFunction)(PyObject *, PyObject *); /** A standard function pointer **/

struct PyMethodDef {
    const char  *ml_name;   /* The name of the built-in function/method */
    PyCFunction  ml_meth;   /* The C function that implements it */
    int      ml_flags;  /* Combination of METH_xxx flags, which mostly
                   describe the args expected by the C func */
    const char  *ml_doc;    /* The __doc__ attribute, or NULL */
};

typedef struct {
    PyObject_HEAD
    PyMethodDef *m_ml; /* Description of the C function to call */
    PyObject    *m_self; /* Passed as 'self' arg to the C func, can be NULL */
    PyObject    *m_module; /* The __module__ attribute, can be anything */
} PyCFunctionObject;
```

We can see that a `PyCFunctionObject` is just a C function pointer with some meta-data.  
Here is the `ctypes` structure that represents those two structs:

```
class PyMethodDef(ctypes.Structure):
    _fields_ = [("ml_name", ctypes.POINTER(ctypes.c_char)), ("ml_meth", ctypes.c_void_p),
            ("ml_flags", ctypes.c_int), ("ml_doc", ctypes.POINTER(ctypes.c_char))]

# Reuse of PyObj from the first example
class PyCFunctionObject(PyObj):
    _fields_ = [("m_ml", ctypes.POINTER(PyMethodDef)),
        ("m_self", ctypes.c_void_p), ("m_module", ctypes.c_void_p)]

# The only new thing is ctypes.POINTER.
# This represent a pointer to another type, the
# content can be accessed by using `.contents`

>>> mabs = PyCFunctionObject.from_address(id(abs))
>>> mabs.m_ml.contents.ml_name[0:3]
'abs'
```

It seems to work. Now we need to see if code execution works too.

```
# Launching Python into GDB to nicely handle the crash
$ gdb -q -args `which python` -i intro_type.py
...
# Get the address of the builtin `abs`
(gdb) x/i 'builtin_abs.32990'
   0x51c8f0 <builtin_abs.32990>:        sub    $0x8,%rsp 
(gdb) r
...
>>> # repeat code of last example
# See if the `abs` C code match with what we found in gdb
>>> mabs.m_ml.contents.ml_meth
'0x51c8f0'
>>> mabs.m_ml.contents.ml_meth = 0x42424242
>>> abs(-1)
# Yeah!
Program received signal SIGSEGV, Segmentation fault.
0x0000000042424242 in ?? ()
(gdb)
```

That was easy! but this part was the simple one. If we want to get reliable code execution many steps remain.

### Executing our code

The next step is to actually execute some of our code. For that, I chose the `mmap` module to create executable pages and stuff them with code and data.

There is only two problems with the `mmap` module:

- the `mmap` type doesn't expose the actual address of the page to Python (as usual we will use `ctypes` to figure that out),
- the `mmap` "API" is not the same for Linux and Windows (I have simply created my own platform-independant wrapper for that).

How to get the real address of the mmaped page? same procedure as usual:

```
typedef struct {
    PyObject_HEAD
    char *      data;
    size_t      size;
    size_t      pos;    /* relative to offset */
    ... /** Other fields that are non-relevant to our goal **/
} mmap_object;
```

All we need is the first field, which points to our page. In the following examples, all `PyObj` that I describe can be found in the file `intro_type.py`.

```
import intro_type
import mmap
# Ctypes object for introspection
class PyMmap(intro_type.PyObj):
    _fields_ = [("ob_addr", ctypes.c_size_t)]

# Specific mmap class for code injection
class MyMap(mmap.mmap):
    """ A mmap that is never unmaped and that contains the page address """
    def __init__(self, *args, **kwarg):
        # Get the page address by introspection of the C struct
        m = PyMmap.from_address(id(self))
        self.addr = m.ob_addr
        # Prevent garbage collection (and so the unmapping) of the page
        m.ob_refcnt += 1

    @classmethod
    def get_map(cls, size) 
    """ Returns a page of size 'size': the implementation is not important for our example.
        See the repository if you want the actual code. ;)
    """
```

Now, to be sure that our first step (simple code execution) works, we will replace the `abs` builtin by an `identity` one, equivalent to `lambda x : x`.

I run a `64 bits` Python so the example of native code here is 64 bits, if you want the 32 bits version: look at the repository. ;)  
The payload used for identity is:

```
    # All ctypes structs and MyMap classes are in the intro_type.py file.
    import intro_type

    # RSI is the first argument we pass and RAX is the return value.
    # So we just return our first argument.

    # Normally, the first argument of a function is RDI but builtins receive a 'self' as the actual first argument.
    payload = "4889F0" + "C3"  # mov rax, rsi ; ret
    payload = payload.decode('hex')

    # Get a page.
    page = intro_type.MyMap.get_map(0x1000)
    # Put our payload at the beginning.
    page[0:len(payload)] = payload
    mabs = intro_type.PyCFunctionObject.from_address(id(abs))
    # Change the function pointer to the address of our payload.
    mabs.m_ml.contents.ml_meth = page.addr

    >>> abs(4)
    4
    # Well it doesn't segfault: good news!
    >>> abs(-4)
    -4 
    # Well this is not the real abs anymore: another good news!
    >>> x = (1,2,3)
    >>> abs(x)
    (1,2,3)
    >>> abs(x)
    <refcnt 0 at 0x7f48b121eaa0>
    # Yep, do not forget that our function doesn't increment the refcount of the returned object.
    # This version can lead to crash!
```

Perfect, our dummy code execution works!

## Object hand-crafting

The next step of our journey is the hand-crafting of some Python objects.  
Why? because it's fun and it will be useful for our final goal!

### Writing raw objects in memory

The first step of object crafting is to write the object data in memory.  
For that, we are going to use the `struct` module (another really useful module). In this example we write the data for an `int` object (in 64 bits Python):

```
/** here is the struct that represents an int **/
typedef struct {
    PyObject_HEAD
    long ob_ival;
} PyIntObject;
```

So:

```
import struct

>>> help(struct.pack_into)
pack_into(...)
   Pack the values v1, v2, ... according to fmt.
   Write the packed bytes into the writable buffer buf starting at offset.
# This module heavily uses the struct format.
# Doc is here: https://docs.python.org/2/library/struct.html.

# Write three unsigned long long in little endian at offset 0 of 'page'.
>>> struct.pack_into("<QQQ", page, 0,
        2,       #ob_refcnt
        id(int), # ob_type            
        42)      # ob_ival

# That's all!
# Problem is: I have no way to prove you that it works before the next sub-step. :(
# Trust me!
```

The problem is: even if we have crafted a valid `int` in memory, it is not usable since the Python interpreter as no reference to it.  
The next step is to create a reference to our crafted objet.

### Giving life to the crafted object

Here is the problem:

- we have a valid object in memory,
- we have its address,
- the Python interpreter have no reference to it,
- we need to create the reference to give life to our object.

All we have to do is call a function that can return arbitrary valid references.

The easiest way is to return the address (so the object) corresponding to the value of the `int` we receive as parameter.  
Think of it as the reverse `id` function:

```
# How our function should work:
>>> x = (1,2,3)
>>> id(x)
140147953449472
>>> my_func(140147953449472)
(1,2,3)
```

If this description is not enough, here is what the C code should be:

```
PyObject* my_func(PyIntObject* i)
{
    return (PyObject *) i->ob_ival;
}
/** Here we can see can the function can be harmful if wrongly used **/
```

So, here are the payloads for this function:

```
# int-to-object payloads:

# 64bits
# mov rax, [rsi + 0x10] # rsi is the address of the int; [rsi + 0x10] the ob_ival of the int
# ret
bootstrap_64 = "48 8b 46 10 C3".replace(" ", "").decode("hex")

# 32bits
# mov eax, [esp + 8] # [esp + 8] -> builtin argument
# mov eax, [eax + 0x8] # [eax + 14] ob_ival
# ret
bootstrap_32 = "8B 44 24 08 8B 40 08 C3".replace(" ", "").decode("hex")
```

Putting everything together, we get:

```
import struct
import intro_type

# Write our object into a page.
>>> page = intro_type.MyMap.get_map(0x1000)
>>> struct.pack_into("<QQQ", page, 0, 2, id(int), 42)
# Put the payload just after our int object in the page.
>>> bootstrap_64 = "48 8b 46 10 C3".replace(" ", "").decode("hex")
>>> page[24:24 + len(bootstrap_64)] = bootstrap_64
# Replace abs by our function.
>>> mabs = intro_type.PyCFunctionObject.from_address(id(abs))
>>> mabs.m_ml.contents.ml_meth = page.addr + 24
# Give life to our int!
>>> x = abs(int(page.addr))
#IT'S ALIVE!!
>>> x
42
# Proof that our int is not the original 42 (addresses comparaison).
>>> x is 42
False
# Proof that we have an object at the beginning of our page.
>>> hex(id(x))
'0x7ffff7ff8000L'
>>> hex(page.addr)
'0x7ffff7ff8000L'
```

We are now able to create Python objects out of the great void! We are near our goal.

## Reliable native code execution

The problems with the previous dummy code execution are:

- we have a limited number of different functions (limited by the number of existing builtins),
- each time we create a new dummy-execution we lose a builtin,
- we put the interpreter in an instable state as any part of the code could potentially use a replaced buitlin without knowing.

With the tools we acquired previously, the obvious solution to these problems is: **we are going to craft our own builtin objects!**

By doing so, we can have any number of different native functions without putting the interpreter in an unstable state by replacing builtins.

So here are the steps to create a complete builtin object:

- Write our payload on a page.
- Write a `PyMethodDef` on the page with the following values:
    
    - `ml_name`: address of a C string (using `ctypes.addressof(ctypes.create_string_buffer(name))`),
    - `ml_math`: address of the payload,
    - `ml_flags`: `8` for `METH_O` (method with a simple object, can be changed depending of our need),
    - `ml_doc`: `0` (no need to document so `None` is cool) or address of a C string.
- Write a `PyCFunctionObject` on the page with the following values:
    
    - `ob_refcnt`: `0x42` (we really don't want it to be collected since not allocated by Python),
    - `ob_type`: `<type 'builtin_function_or_method'>` so `id(type(abs))` is fine,
    - `m_ml`: address of `PyMethodDef`,
    - `m_self`: `0`,
    - `m_module`: `0` (or any `PyObj` address).
- Give life to our object (see previous section).
    

Here is a demo for `x86_64`:

```
import struct
import intro_type 
def create_builtin(code, name="crafted builtin"):
    """ Dummy implementation of 'create_builtins' for x86_64"""
    # Create page.
    m = intro_type.MyMap.get_map(0x1000)
    # Get a C string with the name.
    cname = ctypes.create_string_buffer(name)
    name_addr = ctypes.addressof(cname)
    # Write the payload on the page.
    m[0:len(code)] = code
    # Write the PyMethodDef on the page.
    struct.pack_into("<QQQQ", m, 0x100,
        name_addr, m.addr, 8, name_addr)
    # Write the PyCFunctionObject on the page.
    struct.pack_into("<QQQQQ", m, 0x200,
        42, id(type(min)), m.addr + 0x100, 0, 0)
    # Give life to our object.
    return give_life_to_obj(int(m.addr + 0x200))

# Create page with our `give_life_to_obj` payload.
code_page = intro_type.MyMap.get_map(0x1000)
bootstrap_64 = "48 8b 46 10 C3".replace(" ", "").decode("hex")
code_page[0:len(bootstrap_64)] = bootstrap_64
# Replace abs by our function.
mabs = intro_type.PyCFunctionObject.from_address(id(abs))
mabs.m_ml.contents.ml_meth = code_page.addr
give_life_to_obj = abs
# A builtin that trigger a breakpoint when executed.
breakpoint = create_builtin("xcc", "breakpoint")

>>> breakpoint
<built-in function breakpoint>
>>> breakpoint.__name__
'breakpoint'
>>> breakpoint(None)
[1]    4870 trace trap (core dumped)  python -i demo.py
# woot! breakpoint triggered o/
```

The only problem now is the bootstrap of `give_life_to_obj`: to give life to a new builtin object we would need to already have the dummy execution. So we are going to:

- replace `abs` by our `give_life_to_object` payload,
- create our first builtin: `real_give_life_to_object`,
- put back the original code of `abs`,
- use our new builtin to create the builtin objects.

Bootstrap code:

```
def bootstrap():
    payload_64 = "48 8b 46 24 C3".replace(" ", "").decode("hex")
    # 'replace_builtin' replaces the code of its first argument and returns the old 'ml_meth' for restoration.
    old_abs = replace_builtin(abs, payload_64)
    # In my final code 'give_life_to_object' is named 'get_obj_at'.
    get_obj_at = abs
    new_obj_at = self.create_builtin(payload_64, "get_obj_at")
    # Just restore old code of builtin abs.
    restore_builtins(abs, old_abs)
    get_obj_at = new_obj_at
```

With that, we are able to create all the builtins we want!

### Windows/Linux and 32/64 bits compatibility

Before releasing the code I worked at getting a working Linux and Windows version for both the 32 and 64 architectures.  
Here are what I had to do:

Windows/Linux:

- No change except MyMap class (mmap wrapper).
- Get the OS using `platform.system()`.
- Call `mmap` with correct arguments depending on the system.

32/64 bits:

- Get the architecture using: `platform.architecture()[0]`.
- Adapt:
    
    - the payload for `obj_at` (describe as `give_life_to_obj` in the article),
    - the format strings for `struct.pack_into`.

Last problem I had was with the way I gave life to object. It doesn't work very well on 32 bits systems because `int(addr)` may return a `long`, which is not compatible with our payload.

So in the final version, I use the `str` type and a string such as `"x41x42x43x44"` to pass the address of the object I want to give life to. This only change the way the payload is called, and how the offset the payload is looked for in the object.

### Final result

The final result is pretty simple, all the magic is put into the `CodeInjector` class in the file `code_injection.py`. Just use it like this:

```
>>> import code_injection
>>> code_injection.CodeInjector
<class 'code_injection.CodeInjector'>
>>> ci = code_injection.CodeInjector()
>>> b = ci.create_builtin("xcc", "my_breakpoint_builtin")
>>> b
<built-in function my_breakpoint_builtin>
```

### Conclusion

I had a LOT of fun creating new objects and having Python interface so nicely with my own native payloads. Of course, this kind of horrible stuff (Pythonicly speaking) should not be used in real projects...  
I hope you enjoyed this article and learned something (I did !).  
Also, `struct` is a super-useful module for low level stuff! I discovered it during my first CTF and use it at almost every CTFs now.

The complete implementation is avalaible at https://bitbucket.org/Hakril/python-native-execution/.

Go to Source

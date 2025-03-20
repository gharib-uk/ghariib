---
title: "Understanding Python by breaking it"
date: 2025-01-19
categories: 
  - "cybersecurity"
  - "cybersecurity-awareness"
  - "security"
  - "security-awareness"
  - "vulnerabilities"
tags: 
  - "define"
---

I have recently discovered `ctypes` and decided to use it to manipulate Python in a way I should not. The purpose was to see some of the _Python's internals_ at work directly from the interpreter.  
The article studies to the `CPython` implementation of Python 2.7.

## Reference counting and the Garbage Collector

One thing to know about Python is that its Garbage Collector uses reference counting. It means that every Python object has a _ref counter_ that knows how many references are pointing to the object.  
Our first goal is to visualise that _ref counter_ for any object from the Python interpreter.

The principal tool we are going to use is:

- **`ctypes._CData.from_address`** that allows us to create a `ctypes` mapping on any address we want:

Example (a bad one)

```
import ctypes
ctypes.c_size_t.from_address(0)
# segfault! (to dereference 0 is not a good idea)
```

So we now have the ability to read any value in our address space. Next step is to get an object's address. For that, we will use a builtin function:

- _`id()`_ : returns the “identity” of an object. This is an integer (or long integer) which is guaranteed to be unique and constant for this object during its lifetime.  
    **`CPython` implementation detail**: This is the address of the object in memory.

All we need to do now is to get the offset of the _ref counter_ in the Python object:

```
/** My own comments begin by '**' **/
/** From: Includes/object.h **/

typedef struct _object {
    PyObject_HEAD
} PyObject;

/* PyObject_HEAD defines the initial segment of every PyObject. */
#define PyObject_HEAD    
    _PyObject_HEAD_EXTRA   /** empty in standard build **/ 
    Py_ssize_t ob_refcnt;  /**ref counter **/ 
    struct _typeobject *ob_type; /** pointer to the type **/
```

We can see that `ob_refcnt` is the first field of the struct, and therefore `addr(ob_refcnt) == addr(object) == id(object)`.

So our function will simply be :

```
def get_ref(obj):
    """ returns a c_size_t, which is the refcount of obj """
    return ctypes.c_size_t.from_address(id(obj))
```

Let's try it!

```
>>> import ctypes
>>> def get_ref(obj):
...     """ returns a c_size_t, which is the refcount of obj """
...     return ctypes.c_size_t.from_address(id(obj))
...
# the c_ulong is not a copy of the address
# so any modification of the ob_refcnt are directly visible
>>> l = [1,2,3,4]
>>> l_ref = get_ref(l)
>>> l_ref 
c_ulong(1L) # there is just one reference on the list (l)

>>> l2 = l
>>> l_ref 
c_ulong(2L)# two references on the list (l and l2)

>>> del l
>>> l_ref
c_ulong(1L)

>>> del l2
>>> l_ref
c_ulong(0L) # no more reference!

>>> another_list = [0, 0, 7]
>>> l_ref
c_ulong(1L) # woot : old list's ob_refcnt have changed
```

This example is pretty straightforward, but the last two lines need an explanations. Why would creating a new list change the _ref counting_ of the old one ?  
This is the work of the Garbage Collector! When the `old_list` _ref count_ go to 0, the GC "cleans" the list and put it in a pool of unused lists that will be used the next list to be created!

Proof:

```
>>> l1 = [1,2,3]
>>> l2 = [1,2,3]
>>> hex(id(l1))
'0xa367e8'
>>> hex(id(l2))
'0xa36d40'  # not the same address as l1 (hopefully)
>>> del l1  # l1 is garbage collected
>>> l3 = [8,0,0,8,5]
>>> hex(id(l3))
'0xa367e8'  # l1 address is reused for the new list l3
```

### Special case : `int` and `str`

In the `CPython` implementation of `int`, the references to `[-5 ; 256]` are shared. And we have the tools to verify that!

```
# with "non-shared" int
>>> x1 = 257
>>> x2 = 257
>>> get_ref(x1)
c_ulong(1L)
>>> get_ref(x2)
c_ulong(1L)
>>> id(x1) == id(x2)
False  # logic : differents objects

>>> x3 = 0
>>> get_ref(x3)
c_ulong(409L) #  all ref to "0" point the the same "int(0)" object
```

Same thing happens with strings!

```
>>> p = "python"
>>> get_ref(p)
c_ulong(8L)
>>> p2 = "python"
>>> get_ref(p2)
c_ulong(9L)
>>> id(p) == id(p2)
True  # the two variables are references to the same string object!
```

### Breaking reference counter

For now, we have just read the value of `ob_refcnt`. What if we change it ?  
We can rewrite `ob_refcnt` to force a premature garbage collection!

```
>>> x = [1,2,3,4]
>>> x_ref = get_ref(x)
>>> xx = x
>>> x_ref
c_ulong(2L)
>>> x_ref.value = 1 # GC now thinks that we have just one reference to the list
>>> del xx # ob_refcnt == 0 -> garbage collection!
>>> x
[] # garbage collection of a list sets its size to 0
>>> another_list = [0, 4, 8, 15 ,16, 23, 42]
>>> x
[0, 4, 8, 15 ,16, 23, 42]  # always the same "reuse list" tricks!
```

Of course: this kind of code put the interpreter into an unstable state that will likeky cause crashes!

## Messing with tuple

Other fun and simple things to mess with, are tuples. The documentation says "_`tuple` is an immutable sequence type_" : let's try to change this!

If you look at the `CPython` implementation you can find:

```
/** From: Includes/tupleobject.h **/
typedef struct {
    PyObject_VAR_HEAD
    PyObject *ob_item[1]; /** An array of pointer to PyObject **/
} PyTupleObject;

/** From: Includes/object.h **/
#define PyObject_VAR_HEAD     
    PyObject_HEAD    /** See: part 1 **/ 
    Py_ssize_t ob_size; /* Number of items in variable part */
```

So the two importants things about tuples are:

- a tuple is basically just an array of pointers to `PyObject` and,
- a tuple object is composed of three `size_t` (`ref_count`, `ob_type`, `ob_size`) and the array of pointers.

Based on that and the `memmove` implementation in `ctypes` we can build a `tuplecopy` function:

```
def tuplecpy(dst, src, begin_offset):
    """
       Of course this function should NEVER be used in real code
       It  will probably result in segfaults/crashes
       - copy tuple(src) to dst[begin_offset:] tuple
       - remember id(x) -> addressof(x)
    """
    OFFSET = ctypes.sizeof(ctypes.c_size_t) * 3
    PTR_SIZE = ctypes.sizeof(ctypes.c_size_t)
    dst_addr = id(dst) + OFFSET + PTR_SIZE * begin_offset
    src_addr = id(src) + OFFSET
    ctypes.memmove(dst_addr, src_addr, len(src) * PTR_SIZE)
```

Let's try this new toy!

```
>>> x1 = tuple("A" * 4)
>>> x2 = tuple("B" * 2)

>>> print ("BEFORE -> x1 = {0} | x2  = {1}".format(x1, x2))
>>> tuplecpy(x1, x2, 2)
>>> print ("AFTER  -> x1 = {0} | x2  = {1}".format(x1, x2))

#Result:
#    BEFORE -> x1 = ('A', 'A', 'A', 'A') | x2  = ('B', 'B')
#    AFTER  -> x1 = ('A', 'A', 'B', 'B') | x2  = ('B', 'B')
```

It works! but why is this foundamentally bad (besides the fact of modifying tuples)?  
The answer is in the first section: we have created new references to multiple objects (the ones in the src tuple) without any update of their _ref count_

```
>>> x1 = tuple("B" * 4)
>>> x2 = ([1,2,3,4],)

# problem is : 
>>> t = get_ref(x2[0])
>>> t
c_ulong(1L)
>>> del x2 # no more references
>>> x1
('B', 'B', 'B', <refcnt 0 at 0xacac68>)  # nice printing of an object with ob_refcnt == 0 :)

>>> l = [0, 42, 69]
>>> x1 # GC IN ACTION o/
('B', 'B', 'B', [0, 42, 69]) # Same principle as before
```

We can just fix our function so that it updates the reference counter! (but it doesn't mean that we should now use this function in real code..)

```
def tuplecpy(dst, src, begin_offset):
    """
       Of course this function should NEVER be used in real code
       It  will probably result in segfaults/crashes
       - copy tuple(src) to dst[begin_offset:] tuple
       - remember id(x) -> addressof(x)
    """
    OFFSET = ctypes.sizeof(ctypes.c_size_t) * 3
    PTR_SIZE = ctypes.sizeof(ctypes.c_size_t)
    for obj in src:
        x = get_ref(obj)
        x.value = x.value + 1 # manually update ob_refcnt
    dst_addr = id(dst) + OFFSET + PTR_SIZE * begin_offset
    src_addr = id(src) + OFFSET
    ctypes.memmove(dst_addr, src_addr, len(src) * PTR_SIZE)
```

## Diving into function and code objects.

Now that we can modify tuple, we are going to see the impact of modifying some important tuples in function objects and code objects

### Code object

As explained in the documentation : "_Code objects represent byte-compiled executable Python code_"

In Python 2 code objects are in the `func_code` variable of function objects.

```
>>> import dis  # bytecode disassembler module
>>> def time_2(x):
...     return 2 * x
... 
>>> time_2.func_code
<code object time_2 at 0x9ee230, file "<stdin>", line 1>
>>> time_2(x=4)
8
>>> dis.dis(time_2)
          0 LOAD_CONST               1 (2)
          3 LOAD_FAST                0 (x)
          6 BINARY_MULTIPLY
          7 RETURN_VALUE
```

If we look at the disassembly of the function (which is pretty easy to read), we can see that the function:

- `LOAD` the constant `(2)`,
- `LOAD` the variable `(x)`,
- `MULTIPLY` the two values,
- `RETURN` the result.

If we focus on the first instruction (`LOAD_CONST`) we can see the following things:

- `LOAD_CONST` is _called_ with an argument `1`
- that argument points to the const `2`

The fact is that `1` is just an offset into the code object's constants tuple.

```
>>> time_2.func_code.co_consts  # tuple of constants of our code object
(None, 2)
>>> time_2.func_code.co_consts[1]
2 # yep we were right!
# So what if we change this value ?
>>> tuplecpy(time_2.func_code.co_consts, (10,), 1)
>>> time_2.func_code.co_consts # tuple of constants of our code object
(None, 10)
>>> time_2(4)
40 # woot! It works!
```

So we are able to modify the constant used in the function.  
Can we do the same for the `LOAD_FAST` instruction ?

```
>>> time_2(4) # works on the modified function!
40
>>> time_2(x=4) # call by dict
40
>>> time_2.func_code.co_varnames # tuple of local variables and argnames
('x',)
>>> tuplecpy(time_2.func_code.co_varnames, ('new_arg_name',), 0)
>>> time_2(x=4)
# x is not the argument name anymore!
TypeError: time_2() got an unexpected keyword argument 'x'
>>> time_2(new_arg_name=4)
40
>>> dis.dis(time_2) # see the function changes:
          0 LOAD_CONST               1 (10) # const changed
          3 LOAD_FAST                0 (new_arg_name) # arg name changed
          6 BINARY_MULTIPLY
          7 RETURN_VALUE
```

So, yeah, we have modified the behaviour of the function pretty well!  
And here is a last example that I find very fun:

```
>>> def nop(): pass  # the function that does nothing
...
>>> dis.dis(nop)
         0 LOAD_CONST               0 (None)
         3 RETURN_VALUE
>>> nop.func_code.func_consts
(None,)
>>> l = []
>>> tuplecpy(nop.func_code.func_consts, (l,), 0)  # the function will always return the same list!
>>> nop()
[]
>>> l.append(2)
>>> nop()
[2]
>>> dis.dis(nop)
         0 LOAD_CONST               0 ([2])
         3 RETURN_VALUE
```

### Function closure!

Finally, we are going to play with closure! Closure appear in function generating functions. Wikipedia page

```
>>> def gen_return_function(x):
...     def f():
...         return x  # in f(): x is going to be a closure
...     return f
... 
>>> ret_42 = gen_return_function(42)
>>> ret_42()
42
>>> dis.dis(ret_42)
          0 LOAD_DEREF               0 (x)
          3 RETURN_VALUE
```

This new instruction `LOAD_DEREF` is the one that handles closure. The question is: where is the closure stored?  
The answer is simple : closures are in `ret_42.func_closure`. why not in the object code ? Because it allows all generated functions to share the same object code but with different closures!

```
>>> ret_23 = gen_return_function(23)
>>> ret_42 = gen_return_function(42)
>>> ret_42.func_code is ret_23.func_code
True
>>> ret_42.func_closure
(<cell at 0xa54398: int object at 0x95d748>,) # closures are always inside a cell object
>>> ret_42.func_closure[0].cell_contents
42

# We are not going to rewrite the tuple but directly the contents of the cell instead.
# We will still use tuplecpy but with an offset of (-1) because cell have no ob_size.

>>> x = 1337
>>> tuplecpy(ret_42.func_closure[0], (x,), -1)
>>> ret_42()
1337 # closure modified :)
```

That's all!

### Conclusion

I really enjoyed messing with Python (one more time) and I hope you enjoyed it too. I think that this method is a good way to quickly and easily have a look at some Python's internals and how they work.

Lastly, `ctypes` is also a very powerful module to do legitimate work, and it is also capable of loading shared libraries and call C functions. If you have not already used `ctypes`, I strongly recommand you to read the ctypes Python documentation, and give it a try!

Go to Source

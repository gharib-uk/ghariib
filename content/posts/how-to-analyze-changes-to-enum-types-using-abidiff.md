---
title: "How to analyze changes to enum types using abidiff"
date: 2025-02-06
tags: 
  - "include"
---

It is required to have a stable  application binary interface (ABI) when maintaining a stable shared library that is written in C or C++ and shipped as part of a complex software stack. Developers must comply with this requirement. When building a newer version of a shared library, developers may try the following approach:

- Analyze the ABI changes.
- Detect the potential incompatibility of the changes, or ABI breaks.
- Fix them before releasing the library.

To perform the analysis, developers can use the abidiff tool to compare the newer version of the shared library against the previous stable version. The abidiff tool is part of the libabigail framework. It reports the ABI changes to data types and how they impact the functions of variables.

This is a tutorial illustrating the changes to enum types in C and C++, how they are analyzed by libabigail, and how developers can use this analysis to tailor it to their needs.

## Detecting and categorizing enum changes

The libabigail framework reads the debug information accompanying the binary shared library and constructs an internal representation (IR) of the data types in the ABI. Then it compares the IRs of the two binary versions and analyzes the resulting changes.

After comparing the IRs of the enum type, libabigail detects additions or removals of enumerators as well as changes to enumerator names and values. Then it classifies those changes into several categories. The abidiff tool emits an exit code, a bit-field of several categories of changes. By inspecting that exit code for a given reported ABI change, users can see if the change is categorized as an incompatible ABI change (ABI break) or if the tool is suggesting a user review to determine the category of the change.

Usually, abidiff suggests a user review because it doesn't have enough context to determine if the ABI change is compatible or not. In that case, the user provides the missing context so the tool can subsequently categorize similar ABI changes without requiring a review. That context is provided using a suppression specification.

## Example use case

In this section, I will present a use case that is a somewhat realistic example of a shared library that exposes an enum type as part of its ABI. As the hypothetical maintainer of that shared library, I will produce a new version by modifying the enum type in a way that will result in an ABI incompatibility, and I will show you how to use abidiff to detect the ABI change before the release. Then I'll review the ABI change and provide the potentially missing context to abidiff so that subsequent similar ABI changes are better categorized in the future. 

### Defining a libcolor shared library

Let's define a library written in C that gives the red, green, and blue (RGB) components of a given color, following the Red Green Blue color model.

The library defines colors as enumerators of the `enum color_type` type. That enum is defined as follows:

```plaintext
enum color_type
{
  BLACK_COLOR,
  WHITE_COLOR,
  GREEN_COLOR,
  BLUE_COLOR,
  LAST_COLOR
};
```

The library also has a type `struct rgb_type` that corresponds to each color. That `struct` is defined as follows:

```plaintext
struct rgb_type
{
  char red;
  char green;
  char blue;
};
```

Each color denoted by the `enum color_type` corresponds to a given RGB value of type `struct rbg_type`. The RGB value, or RGB code, for a given color is returned by the function `get_color_code` and declared as follows:

```plaintext
struct rgb_type*
get_color_code (enum color_type color);
```

The `display_color` function emits the string representation of a given color (enumerator of `enum color_type`) on the standard output. Likewise, the `display_color_code` function emits the string representation of a given RGB color code (of type `struct rgb_type`) on the standard output.

All of these types and functions are defined or declared in the `libcolor.h` header file which constitutes the application programming interface (API) of the library. The following shows the content of the current version of `libcolor.h`:

```plaintext
$ cat libcolor-v0.h
struct rgb_type
{
  char red;
  char green;
  char blue;
};

enum color_type
{
  BLACK_COLOR,
  WHITE_COLOR,
  GREEN_COLOR,
  BLUE_COLOR,
  LAST_COLOR
};

void
init_color_codes ();

struct rgb_type*
get_color_code (enum color_type color);

struct rgb_type*
set_color_code (enum color_type color, struct rgb_type* color_code);

void
display_color_code (struct rgb_type* color_code);

void
display_color (enum color_type color);
$
```

The shared library is compiled into a `libcolor.so` shared library.

### Creating a colorapp application

For the sake of completeness, let's write a little application that uses the `libcolor.so` shared library through its application programming interface:

```plaintext
cat color-app.c:
#include <stdio.h>
#include "libcolor.h"

int
main()
{
  init_color_codes();

  for (enum color_type color = BLACK_COLOR; color < LAST_COLOR; color++)
    {
      printf ("For color '");
      display_color (color);
      printf ("', the RGB color code is: ");
      struct rgb_type* color_code = get_color_code(color);
      display_color_code (color_code);
      printf (".n");
    }
  return 0;
}
$
```

That code is compiled into the `colorapp` program, dynamically linked against the `libcolor.so` shared library as confirmed by the following output of the `ldd` program on my GNU/Linux system:

```plaintext
$ ldd colorapp 
      linux-vdso.so.1 (0x00007ffe4b3df000)
      libcolor.so (0x00007f8316532000)
      libc.so.6 => /lib64/libc.so.6 (0x00007f8316200000)
      /lib64/ld-linux-x86-64.so.2 (0x00007f8316539000)
$
```

When I execute the `colorapp` program, I get this output:

```plaintext
$ ./colorapp 
For color 'black', the RGB color code is: {Red: 0, Green: 0, Blue: 0}.
For color 'white', the RGB color code is: {Red: 0xf, Green: 0xf, Blue: 0xf}.
For color 'green', the RGB color code is: {Red: 0, Green: 0xf, Blue: 0}.
For color 'blue', the RGB color code is: {Red: 0, Green: 0, Blue: 0xf}.
$
```

### Changing the ABI of the shared library

Looking at the `enum color_type`, I see that the definition of the `RED_COLOR` enumerator is missing. Oops! Let's create a new version of the `libcolor.so` library and amend the `enum color_type` to add a new `RED_COLOR` enumerator. This new version also adds support for the new `RED_COLOR` enumerator in the `get_color_code` and `display_color` functions.

The following shows the textual difference between `libcolor-v0.h` and the newer `libcolor-v1.h` reported by the GNU Diff tool:

```plaintext
$ diff -p -u v0/libcolor-v0.h v1/libcolor-v1.h 
--- v0/libcolor-v0.h
+++ v1/libcolor-v1.h
@@ -9,6 +9,7 @@ enum color_type
 {
   BLACK_COLOR,
   WHITE_COLOR,
+  RED_COLOR,
   GREEN_COLOR,
   BLUE_COLOR,
   LAST_COLOR
$ 
```

Let's build the newer version of `libcolor.so` and name it `libcolor.so.1`.

Please note that the initial `libcolor.so` is a symbolic link that points to the initial `libcolor.so.0`. If we want to use the newer `libcolor.so.1`, we can make the symbolic link `libcolor.so` point to `libcolor.so.1`. For now, here is what we have:

```plaintext
$ ls -l libcolor.so
lrwxrwxrwx. 1 dodji dodji 16 25 nov.  12:08 libcolor.so -> v0/libcolor.so.0
$
```

### Analyzing the resulting ABI change with abidiff

At this point, we can use `abidiff` to compare the ABI of the newer `libcolor.so.1` against the older `libcolor.so.0` as follows:

```plaintext
$ abidiff v0/libcolor.so.0 v1/libcolor.so.1; echo "abidiff returned code: $?"
Functions changes summary: 0 Removed, 1 Changed (2 filtered out), 0 Added functions
Variables changes summary: 0 Removed, 0 Changed, 0 Added variable

1 function with some indirect sub-type change:

  [C] 'function void display_color(color_type)' at libcolor-v0.c:55:1 has some indirect sub-type changes:
    parameter 1 of type 'enum color_type' has sub-type changes:
      type size hasn't changed
      1 enumerator insertion:
        'color_type::RED_COLOR' value '2'
      3 enumerator changes:
        'color_type::GREEN_COLOR' from value '2' to '3' at libcolor-v1.h:8:1
        'color_type::BLUE_COLOR' from value '3' to '4' at libcolor-v1.h:8:1
        'color_type::LAST_COLOR' from value '4' to '5' at libcolor-v1.h:8:1

abidiff returned code: 4
$
```

We see that `abidiff` detects a change to the `enum color_type`. The change is the insertion of the `RED_COLOR` enumerator. But that change also includes changes to the values of the existing enumerators `GREEN_COLOR`, `BLUE_COLOR`, and `LAST_COLOR`.

Please note that the `abidiff` tool returns a code that is `4`. Looking at the documentation for the return codes of abidiff, we see that `4` is the value `ABIDIFF_ABI_CHANGE`. This means that `abidiff` has categorized this ABI change as needing a human review to determine if it's compatible.

The addition of the new `RED_COLOR` enumerator is not an incompatible change. But changing the existing enumerator values `GREEN_COLOR` and `BLUE_COLOR` can be considered as incompatible changes.

By "incompatible", I mean the change can cause an unexpected behavior in an application previously compiled against `libcolor.so.0` that now executes, using the newer `libcolor.so.1` without being recompiled against the newer API of `libcolor.so.1`.

By chance, we do have such an application, the `colorapp` program. Let's execute it against the newer `libcolor.so.1` to see if its behavior changes:

```plaintext
$ rm libcolor.so
$ ln -s v1/libcolor.so.1 libcolor.so
$ ./colorapp 
For color 'black', the RGB color code is: {Red: 0, Green: 0, Blue: 0}.
For color 'white', the RGB color code is: {Red: 0xf, Green: 0xf, Blue: 0xf}.
For color 'red', the RGB color code is: {Red: 0xf, Green: 0, Blue: 0}.
For color 'green', the RGB color code is: {Red: 0, Green: 0xf, Blue: 0}.
$ 
```

We see that this run of the `colorapp` using `libcolor.so.1` returns the RGB color codes for the colors black, white, red, and green, whereas the previous run using `libcolor.so.0` returned the codes for colors black, white, green, and blue. The ABI change definitely changed the behavior of the application in an unexpected manner. This is what I would call an incompatible ABI change, or ABI break.

### Fixing the ABI break

To fix the ABI break, let's consider creating a third version of the library by adding the newer `RED_COLOR` at the end of the `enum color_type` to avoid changing the value of any meaningful enumerator:

```plaintext

$ diff -u -p v0/libcolor-v0.h v2/libcolor-v2.h
--- v0/libcolor-v0.h  2024-11-25 12:05:21.741730539 +0100
+++ v2/libcolor-v2.h  2024-11-25 15:48:21.659597559 +0100
@@ -11,6 +11,7 @@ enum color_type
   WHITE_COLOR,
   GREEN_COLOR,
   BLUE_COLOR,
+  RED_COLOR,
   LAST_COLOR
 };

$
```

The `libcolor.so.2` is the name of the third library version.

Let's use `abidiff` again to compare the ABI of the newer `libcolor.so.2` against the initial `libcolor.so.0`:

```plaintext
$ abidiff v0/libcolor.so.0 v2/libcolor.so.2; echo "abidiff returned code: $?"
Functions changes summary: 0 Removed, 1 Changed (2 filtered out), 0 Added functions
Variables changes summary: 0 Removed, 0 Changed, 0 Added variable

1 function with some indirect sub-type change:

  [C] 'function void display_color(color_type)' at libcolor-v0.c:55:1 has some indirect sub-type changes:
    parameter 1 of type 'enum color_type' has sub-type changes:
      type size hasn't changed
      1 enumerator insertion:
        'color_type::RED_COLOR' value '4'
      1 enumerator change:
        'color_type::LAST_COLOR' from value '4' to '5' at libcolor-v2.h:8:1

abidiff returned code: 4
$ 
```

Here we see that the value of the `LAST_COLOR` enumerator of `enum color_type` is changed by addition of the `RED_COLOR` enumerator near the end of the enum.

However, as the library maintainer carefully reviewing the code, I know that the `LAST_COLOR` enumerator is not used in the library in a way that would incur an incompatible behavior change when the `LAST_COLOR` is incremented. Thus, `abidiff` should consider the change to the `LAST_COLOR` color enumerator as harmless.

### Teaching abidiff to ignore the change

We can teach the libabigail framework to suppress (or ignore) some ABI changes. In this case, we want the `abidiff` tool to ignore the last enumerator of the `color_type` enum called `LAST_COLOR`.

Following the documentation of type suppression specifications, we can write the following suppression specification type:

```plaintext
$ cat v2/last-enumerator.suppr
[suppress_type]
# We want to suppress a change to an enum type ...
  type_kind = enum
# ... named 'color_type'
  name = color_type
# ... where the actual change is the value of
# the enumerator named 'LAST_COLOR'
  changed_enumerators = LAST_COLOR
$
```

Let's see how `abidiff` behaves when provided with this suppression specification:

```plaintext
$ abidiff --suppr v2/last-enumerator.suppr v0/libcolor.so.0 v2/libcolor.so.2; echo "abidiff returned code: $?"
Functions changes summary: 0 Removed, 0 Changed (3 filtered out), 0 Added functions
Variables changes summary: 0 Removed, 0 Changed, 0 Added variable

abidiff returned code: 0
$
```

Now the tool says that the ABIs of the first and latest versions of `libcolor.so` are compatible.

Let's test the `colorapp` program by making it use the `v2/libcolor.so.2` shared library and making the `libcolor.so` symbolic link point to `v2/libcolor.so.2` as follows:

```plaintext
$ rm libcolor.so
$ ln -s v2/libcolor.so.2 libcolor.so
$ ls -l libcolor.so
lrwxrwxrwx. 1 dodji dodji 16 26 nov.  00:05 libcolor.so -> v2/libcolor.so.2
$ 
```

Execute the `colorapp` program as follows:

```plaintext
$ ./colorapp 
For color 'black', the RGB color code is: {Red: 0, Green: 0, Blue: 0}.
For color 'white', the RGB color code is: {Red: 0xf, Green: 0xf, Blue: 0xf}.
For color 'green', the RGB color code is: {Red: 0, Green: 0xf, Blue: 0}.
For color 'blue', the RGB color code is: {Red: 0, Green: 0, Blue: 0xf}.
$ 
```

We see that the application's behavior, using the latest version of the library, is the same as the first version. This confirms the last ABI change is now recognized as compatible by `abidiff`.

This suppression specification is very specific, however. As an improvement, we would like a suppression specification that is more generic.

Let's observe that in this code base, every single enum's last enumerator will start with the sub-string `LAST_`. As a maintainer of this code base, that's a coding rule that I am making up. Yes, there have to be advantages to being a maintainer!

It's not uncommon to see open source projects with similar coding standards. Some projects would prefer to have the name of the last enumerator of enum types end up with sub-strings, such as `_LAST`, `_MAX`, `_NBITS`.

We can use the libabigail's type suppression specifications supporting the `changed_enumerators_regexp` property to recognize such patterns,  as follows:

```plaintext
$ cat v2/last-enumerator-2.suppr
[suppress_type]
# We want to suppress a change to any enum type ...
  type_kind = enum

# ... where the actual change is the value of
# any enumerator which named starts with "LAST_"
  changed_enumerators_regexp = ^LAST_.*
$ 
```

Using that updated suppression specification with `abidiff` yields the following output:

```plaintext
$ abidiff --suppr v2/last-enumerator-2.suppr v0/libcolor.so.0 v2/libcolor.so.2; echo "abidiff returned code: $?"
Functions changes summary: 0 Removed, 0 Changed (3 filtered out), 0 Added functions
Variables changes summary: 0 Removed, 0 Changed, 0 Added variable

abidiff returned code: 0
$ 
```

## Conclusion

Detecting non-compatible ABI changes in shared library types is often not a black or white matter. It is frequently necessary for the case to have a human review of the code base to determine if a particular change detected at the binary level is compatible.

This need for human review is due to limitations of the tool. For instance, the tool might have mis-categorized a given ABI change. In that case, it's a bug that should be reported and fixed. In other cases, the context necessary for the tool to properly categorize the ABI change might be lost in the compilation process. In those cases, after the human review, a somewhat equivalent context can be fed to the tool using a suppression specification file for that particular shared library.

Maintainers of stable libraries are encouraged to provide suppression specification files to compare their ABIs before the release of a newer version.

If you have any questions or follow-up request concerning libabigail, please get in touch. The community will be glad to hear from you.

The post How to analyze changes to enum types using abidiff appeared first on Red Hat Developer.  
  

Go to Source

---
title: "How to delete a file whose name begin with “-” dash"
date: 2025-01-20
---

Today, I found a file on my server named `-config.ini` that starts with a hyphen. It seems someone created it as a backup. The problem is, the starting hyphen is treated as a command-line option, making it tricky to delete. If you’re facing the same issue, don’t worry—this guide will show you how to remove it safely.

Files with a “-” at the beginning of their name can be removed using the rm command with additional care since the `-` is interpreted as an option by most Unix commands.

## Delete file name begin with hyphen

Here’s how you can safely remove such files using `--` to stop option parsing

```bash

rm -- -config.ini
```

The `--` tells rm to stop interpreting anything that follows as an option, treating `-config.ini` as a file name.

## Using Relative Path

Another way is to specify the file using its relative or absolute path:

```bash

rm ./-config.ini
```

This approach works because the `./` explicitly indicates the file in the current directory.

## Confirm Before Deleting

If you’re not sure and want to double-check the file before deleting:

```bash

ls -- -config.ini
```

## Conclusion

Deleting files that start with a hyphen may seem tricky at first, but it’s actually quite simple when you know the right steps. By using the methods in this guide, you can safely remove such files without any trouble. I hope this helps you solve the problem easily!

The post How to delete a file whose name begin with “-” dash appeared first on TecAdmin.

Go to Source

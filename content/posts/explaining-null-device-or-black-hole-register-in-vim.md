---
title: "Explaining Null Device or Black Hole Register in Vim"
date: 2025-01-07
---

The aim of this page📝 is to explain the concept of registers in Vim, particularly the black hole register, and its comparison to `/dev/null` in Bash. I am solving an incident where our design has something similar in event streaming context — we have a stream that works like a null device and we write data there in when it has to be written but it is just to discard it. In general, this is known as null device

- Vim has a special register called the black hole register.
- The black hole register discards data without storing it.
- Use `_` (underscore) to specify the black hole register.
- Example commands: `"_d` (delete without storing), `"_x` (delete character) or `"_C"` to change the whole line without yanking its contents to a register
- This concept is similar to redirecting to `/dev/null` in Bash.
- `/dev/null` discards data without saving it.

Example in Bash:  

```
echo "This won't be seen" > /dev/null
```

Example in Vim:  

```
"_d
"_x
"_C
```

## LINKS

- https://vimhelp.org/change.txt.html#copy-move
- https://www.digitalocean.com/community/tutorials/dev-null-in-linux
- https://en.wikipedia.org/wiki/Dev/null

Go to Source

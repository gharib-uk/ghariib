---
title: "List top 10 CPU using processes in Linux"
date: 2025-02-06
---

With the help of the following command, you can easily find top 10 processes that are consuming high CPU resources on any Linux system. `ps` is the default command available in most of Linux operating system that provide details about processes in system.

```
ps -eo pid,cmd,%mem,%cpu --sort=-%cpu | head
```

Output will print top 10 processes that are consuming high cpu on your system in descending order. You can also use top command to filter processes by cpu utilization.

The post List top 10 CPU using processes in Linux appeared first on TecAdmin.

Go to Source

---
title: "List top 10 memory using processes in Linux"
date: 2025-03-19
---

With the help of the following command, you can easily find top 10 processes that are consuming high memory on any Linux system. `ps` is the default command available in most of Linux operating system that provide details about running processes and resource utilization details in the system. Open a terminal and execute following command:

```
ps -eo pid,cmd,%mem,%cpu --sort=-%mem | head
```

Output will print top 10 processes that are consuming high memory on your system in descending order. You can also use top command to filter processes by memory utilization.

## Example: Using ps command

Here is the sample output of ps command that shows top 10 memory consuming process on your system.

```
ps -eo pid,cmd,%mem,%cpu --sort=-%mem | head
```

<figure>

![Top 10 memory using processes in Linux](https://tecadmin.net/wp-content/uploads/2025/02/linux-top-memory-consuming-processes.png)

<figcaption>

Top 10 memory consuming processes

</figcaption>

</figure>

## Example: Using top command

Alternatively, you can also use top command and sort the processes in order by memory utilization.

1. Open a terminal.
2. Type `top` command then hit enter.
3. Press `shift + m` to sort the processes by memory (%MEM) usage.
    
    <figure>
    
    ![Sort processes by memory uses in top command](https://tecadmin.net/wp-content/uploads/2025/02/top-sort-processes-by-memory-uses.png)
    
    <figcaption>
    
    Sort processes by memory uses in top command
    
    </figcaption>
    
    </figure>
    

## Conclusion

In this tutorial, you have learned about `ps` and `top` commands to identify processes that are consuming high memory in your system. This will help you to keep your system running smoothly.

The post List top 10 memory using processes in Linux appeared first on TecAdmin.

Go to Source

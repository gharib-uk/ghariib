---
title: "Search Which Files include Given Text/String in Linux (Examples)"
date: 2025-01-10
---

![](https://ubuntuhandbook.org/wp-content/uploads/2022/03/terminal-logo-250x250.webp)

This tutorial shows how to search and print which files contain your specified text or string in Ubuntu and other Linux in command line, with examples.

Search file or file content is an important skill for Linux administrator. I’ve shown how to use locate command to search files through the keyword in its file-name, path, and file type.

Here I’m going to show you how to search files if you know any text or string they contain.

### Method 1: Use grep command

`grep` is a very popular command line tool that most Linux includes it out-of-the-box. I use it quite often to filter content in command output.

In my case, building an app sometimes work in recent Ubuntu but NOT for older Ubuntu. It outputs error “has no member or class named ‘xxxxxx'”. I need to find out which file contains that member/class in recent Ubuntu and backport to older Ubuntu version.

1\. In the case, I can run the command below, for example, to **search files under `/usr/include` directory which include string `GParamSpecClass` in file content**.

```
grep -lr "GParamSpecClass" /usr/include/
```

Here you may replace the string and searching directory accordingly, or skip `/usr/include/` to search under current working directory.

And, the command options are:

- `-l` or `--files-with-matches` means only print names of files containing matches.
- `-r` or `--recursive`, also search sub-folders for given directory.

As you know, Linux is case sensitive. To ignore, use `-i` or `--ignore-case` in command. So, the commands below work same as last:

```
grep -lir "gparamspecclass" /usr/include/
```

```
grep -lir "GPARAMSPECclass" /usr/include/
```

![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/grep-listfiles-match-700x269.webp)

2\. For choice, you may **print names of files, matched lines, as well as line numbers** of files contain “GParamSpecClass” under /usr/include/, by running similar command below:

```
grep -irn "GParamSpecClass" /usr/include/
```

As you see, in command:

- Skip `-l` or `--files-with-matches`, then it prints matched lines in output.
- Add `-n` or `--line-number`, will also show the line numbers.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/grep-listfiles-lines-700x381.webp)

3\. To show more file content, you may add:

- `-A NUM` or `--after-context=NUM` to print NUM lines of trailing context after matching lines.
- `-B NUM` or `--before-context=NUM` to print NUM lines of leading context before matching lines

For example, search files under `/usr/include` directory that include “`g_cclosure_marshal_generic`” string, print files names, matched lines, as well as 2 lines before and 4 lines after matches.

```
grep -irn -B 2 -A 4 "g_cclosure_marshal_generic" /usr/include/
```

Sometimes, there may be multiple files or multiple lines match what you search. In the case, you may add `-m NUM` or `--max-count=NUM` to search first NUM matches.

For example, the command below will do same as the last, but only show the first one that matches.

```
grep -irn -m 1 -B 2 -A 4 "g_cclosure_marshal_generic" /usr/include/
```

![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/grep-beforeafter-first-700x502.webp)

`grep` supports regex expressions to make searching more flexible. Instead of typing the exact string or text you want to search, you may use “`^string`” to search files contain `string` at beginning of lines, use “`string$`” to search files contain the string in end of lines, or `stringA|stringB` to search files contain either `stringA` or `stringB`.

They are so many regex patterns that I cannot include them all in this tutorial, see wikipedia page for more.

### Method 2: Use ag command

It’s hard for me to remember infrequently used command options. In the case, I tend to use alternatives that just works with default arguments. `ag` is the one for searching file content.

1\. First, **run command to install ag**.

- For Debian, Ubuntu, Linux Mint, use command:
    
    ```
    sudo apt install silversearcher-ag
    ```
    
- For Fedora, RHEL, etc, run command:
    
    ```
    sudo dnf install the_silver_searcher
    ```
    
- While, Arch based systems may use the command below instead:
    
    ```
    sudo pacman -S the_silver_searcher
    ```
    

2\. Then, for example **search files under `/usr/include` that include string “`gparamspecclass`“**:

```
ag "gparamspecclass" /usr/include
```

As the screenshot shows, it prints all the names of files, lines and line numbers that matches. And, it’s case insensitive.

![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/ag-search-700x378.webp)

`ag` command is super fast! In case the default options do not meet your needs, here are some that similar to `grep`:

- `-A NUM` or `--after NUM`, print NUM lines after matches.
- `-B NUM` or `--before NUM`, print NUM lines before matches.
- `-c` or `--count`, print only names of files and the number of matches.
- `-m NUM` or `--max-count NUM`, print only first NUM matches.
- `-f` or `--follow`, also search files link to others out of searching directory.
- `-l` or `--files-with-matches`, only print the names of files containing matches.

For example, search “gparamspecclass” under current directory, and only show file-names and how many matches.

```
ag "gparamspecclass" -c
```

Or search and print the first match, as well as 2 lines before and 4 lines after the match lines.

```
ag -m 1 -A 4 -B 2 "gparamspecclass"
```

![](https://ubuntuhandbook.org/wp-content/uploads/2025/01/ag-search-match-700x338.webp)

`ag` by default excludes files listed in `.gitignore` and `.hgignore` under searching directory, and user can manually add exclusions by create’n’adding them into `.ignore` file.

To also search files mentioned before, either use `-a` option to search all (exclude hidden files) or `--hidden` to search hidden files. For more, see `man ag`.

Go to Source

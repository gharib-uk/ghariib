---
title: "Mastering Grep command in LinuxUnix A Beginners Tutorial"
date: 2025-02-14T00:00:00.000Z
draft: false
type: posts
categories: 
- unix-linux
---
# Mastering Grep command in LinuxUnix A Beginners Tutorial

<br/>

<br/>
[What is Grep](#what-is-grep)[](#what-is-grep)
----------------------------------------------

Grep, short for “global regular expression print”, is a command used for searching and matching text patterns in files contained in the regular expressions. Furthermore, the command comes pre-installed in every Linux distribution. In this guide, we will look at the most common `grep` command use-cases.

[Grep Command in Linux](#grep-command-in-linux)[](#grep-command-in-linux)
-------------------------------------------------------------------------

Grep command can be used to find or search a regular expression or a string in a text file. To demonstrate this, let’s create a text file **_welcome.txt_** and add some content as shown.

```
Welcome to Linux !
Linux is a free and opensource Operating system that is mostly used by
developers and in production servers for hosting crucial components such as web
and database servers. Linux has also made a name for itself in PCs.
Beginners looking to experiment with Linux can get started with friendlier linux
distributions such as Ubuntu, Mint, Fedora and Elementary OS.
```

Great! Now we are ready to perform a few grep commands and manipulate the output to get the desired results. To search for a string in a file, run the command below **Syntax**

```
grep "string" file name
```

OR

````
filename grep "string"
```bash

**Example**:

```bash
grep "Linux" welcome.txt
````

**Output** ![grep command usage](https://journaldev.nyc3.cdn.digitaloceanspaces.com/2018/12/grep-text-example.png) As you can see, grep has not only searched and matched the string “Linux” but has also printed the lines in which the string appears. If the file is located in a different file path, be sure to specify the file path as shown below

```
grep "string" /path/to/file
```

### [Colorizing Grep results using the --color option](#colorizing-grep-results-using-the-color-option)[](#colorizing-grep-results-using-the-color-option)

If you are working on a system that doesn’t display the search string or pattern in a different color from the rest of the text, use the `--color` to make your results stand out. Example

```
grep --color "free and opensource" welcome.txt 
```

**Output** ![grep command usage](https://journaldev.nyc3.cdn.digitaloceanspaces.com/2018/12/grep-color.png)

[Most Common use-cases of `grep` command](#most-common-use-cases-of-grep-command)[](#most-common-use-cases-of-grep-command)
---------------------------------------------------------------------------------------------------------------------------

### [Searching for a string recursively in all directories](#searching-for-a-string-recursively-in-all-directories)[](#searching-for-a-string-recursively-in-all-directories)

If you wish to search for a string in your current directory and all other subdirectories, search using the `- r` flag as shown

```
grep -r "string-name" *
```

For example:

```
grep -r "linux" *
```

**Output** ![grep command usage example](https://journaldev.nyc3.cdn.digitaloceanspaces.com/2018/12/grep-recursive.png)

### [Ignoring case sensitivity](#ignoring-case-sensitivity)[](#ignoring-case-sensitivity)

In the above example, our search results gave us what we wanted because the string “Linux” was specified in Uppercase and also exists in the file in Uppercase. Now let’s try and search for the string in lowercase.

```
grep "linux" file name
```

Nothing from the output, right? This is because grepping could not find and match the string “linux” since the first letter is Lowercase. To ignore case sensitivity, use the `-i` flag and execute the command below

```
grep -i "linux" welcome.txt
```

**Output** ![grep command usage examples](https://journaldev.nyc3.cdn.digitaloceanspaces.com/2018/12/grep-ignore-case-sensitivity.png) Awesome isn’t’ it? **The `- i` is normally used to display strings regardless of their case sensitivity.**

### [Count the lines where strings are matched with -c option](#count-the-lines-where-strings-are-matched-with-c-option)[](#count-the-lines-where-strings-are-matched-with-c-option)

To count the total number of lines where the string pattern appears or resides, execute the command below

```
grep -c "Linux" welcome.txt
```

**Output** ![grep command usage examples](https://journaldev.nyc3.cdn.digitaloceanspaces.com/2018/12/grep-count-number-of-lines.png)

### [Using Grep to invert Output](#using-grep-to-invert-output)[](#using-grep-to-invert-output)

To invert the Grep output , use the `-v` flag. The `-v` option instructs grep to print all lines that do not contain or match the expression. The –v option tells grep to invert its output, meaning that instead of printing matching lines, do the opposite and print all of the lines that don’t match the expression. Going back to our file, let us display the line numbers as shown. Hit ESC on Vim editor, type a full colon followed by

```
 set nu
```

Next, press Enter **Output** ![grep command usage examples](https://journaldev.nyc3.cdn.digitaloceanspaces.com/2018/12/line-number-in-vim.png) Now, to display the lines that don’t contain the string “Linux” run

```
grep -v "Linux" welcome.txt
```

**Output** ![grep command usage examples](https://journaldev.nyc3.cdn.digitaloceanspaces.com/2018/12/grep-invert-output.png) As you can see, grep has displayed the lines that do not contain the search pattern.

### [Number the lines that contain the search pattern with -n option](#number-the-lines-that-contain-the-search-pattern-with-n-option)[](#number-the-lines-that-contain-the-search-pattern-with-n-option)

To number the lines where the string pattern is matched , use the `-n` option as shown

```
grep -n "Linux" welcome.txt
```

**Output** ![grep command usage examples](https://journaldev.nyc3.cdn.digitaloceanspaces.com/2018/12/grep-line-number-where-string-appears.png)

### [Search for exact matching word using the -w option](#search-for-exact-matching-word-using-the-w-option)[](#search-for-exact-matching-word-using-the-w-option)

Passing then `-w` flag will search for the line containing the exact matching word as shown

```
grep -w "opensource" welcome.txt
```

**Output** ![grep command usage examples](https://journaldev.nyc3.cdn.digitaloceanspaces.com/2018/12/grep-search-exact-matching-pattern.png) However, if you try

```
grep -w "open" welcome.txt
```

No, results will be returned because we are not searching for a pattern but an exact word!

### [Using pipes with grep](#using-pipes-with-grep)[](#using-pipes-with-grep)

The grep command can be used together with pipes for getting distinct output. For example, If you want to know if a certain package is installed in Ubuntu system execute

```
dpkg -L | grep "package-name"
```

For example, to find out if OpenSSH has been installed in your system pipe the `dpkg -l` command to grep as shown

```
dpkg -L | grep -i "openssh"
```

**Output** ![grep command usage](https://journaldev.nyc3.cdn.digitaloceanspaces.com/2018/12/grep-package-name.png)

### [Displaying number of lines before or after a search pattern Using pipes](#displaying-number-of-lines-before-or-after-a-search-pattern-using-pipes)[](#displaying-number-of-lines-before-or-after-a-search-pattern-using-pipes)

You can use the **\-A** or **\-B** to dislay number of lines that either precede or come after the search string. The **\-A** flag denotes the lines that come after the search string and **\-B** prints the output that appears before the search string. For example

```
ifconfig | grep -A 4 ens3
```

This command displays the line containing the string plus 4 lines of text after the _ens_ string in the `ifconfig` command. **Output** ![grep usage commands](https://journaldev.nyc3.cdn.digitaloceanspaces.com/2018/12/grep-lines-after-search-string.png) Conversely, in the example below, the use of the -B flag will display the line containing the search string plus 3 lines of text before the _ether_ string in the `ifconfig` command. **Output**

```
ifconfig | grep -B 4 ether
```

![grep command usage](https://journaldev.nyc3.cdn.digitaloceanspaces.com/2018/12/grep-lines-before-search-string.png)

### [Using grep with REGEX - regular expressions](#using-grep-with-regex-regular-expressions)[](#using-grep-with-regex-regular-expressions)

The term REGEX is an acronym for **REG**ular **EX**pression. A REGEX is a sequence of characters that is used to match a pattern. Below are a few examples:

```
^      Matches characters at the beginning of a line
$      Matches characters at the end of a line
"."    Matches any character
[a-z]  Matches any characters between A and Z
[^ ..] Matches anything apart from what is contained in the brackets
```

**Example** To print lines beginning with a certain character, the syntax is;

```
grep ^character file_name
```

For instance, to display the lines that begin with the letter “d” in our welcome.txt file, we would execute

```
grep ^d welcome.txt 
```

**Output** ![grep regex example](https://journaldev.nyc3.cdn.digitaloceanspaces.com/2018/12/regex-beginning-of-line.png) To display lines that end with the letter ‘x’ run

```
grep x$ welcome.txt
```

**Output** ![grep regex end of line](https://journaldev.nyc3.cdn.digitaloceanspaces.com/2018/12/regex-end-of-line.png)

### [Getting help with more Grep options](#getting-help-with-more-grep-options)[](#getting-help-with-more-grep-options)

If you need to learn more on Grep command usage, run the command below to get a sneak preview of other flags or options that you may use together with the command.

```
grep --help
```

**Sample Output** ![grep command usage](https://journaldev.nyc3.cdn.digitaloceanspaces.com/2018/12/grep-help.png) We appreciate your time for going through this tutorial. Feel free to try out the commands and let us know how it went.

### [Using `grep` with Advanced Regex](#using-grep-with-advanced-regex)[](#using-grep-with-advanced-regex)

The `grep` command supports [Extended Regular Expressions (ERE)](/community/tutorials/using-grep-regular-expressions-to-search-for-text-patterns-in-linux#extended-regular-expressions) using the -E flag, also known as `egrep`.

Example:

```
grep -E "error|warning|failure" logfile.txt
```

To search with groups and back-references, use:

```
grep -E "([0-9]{4})-([0-9]{2})-([0-9]{2})" dates.txt
```

For a deep dive into regex patterns, check out this tutoriel on [Using Grep Regular Expressions to Search for Text Patterns in Linux](/community/tutorials/using-grep-regular-expressions-to-search-for-text-patterns-in-linux).

### [How `grep` Handles Multi-threading with GNU Implementation?](#how-grep-handles-multi-threading-with-gnu-implementation)[](#how-grep-handles-multi-threading-with-gnu-implementation)

The [GNU grep](https://www.gnu.org/software/grep/) implementation optimizes searches using multi-threading. If available, grep internally uses AVX2 vectorization to speed up pattern matching.

For multi-threaded searching on large files, you can use:

```
grep --mmap "pattern" largefile.txt
```

Or leverage parallel execution with `xargs`:

```
find /logs/ -type f | xargs -P 4 grep "error"
```

This allows `grep` to process multiple files in parallel, improving performance on multi-core systems.

### [Common Pitfalls with Recursive Searches in `grep`](#common-pitfalls-with-recursive-searches-in-grep)[](#common-pitfalls-with-recursive-searches-in-grep)

When using `grep -r`, be mindful of:

1.  Symbolic Links Loops: Use `grep -R` to follow symbolic links safely.
    
2.  Binary File Output: Use `grep --binary-files=text` to process binaries as text.
    
3.  Permission Errors:Run `grep` with `sudo` for restricted directories.
    

Example of a safer recursive search:

```
grep -r --exclude-dir={proc,sys,dev} "error" /
```

[FAQs](#faqs)[](#faqs)
----------------------

### [1\. What is the `grep` command in Linux?](#1-what-is-the-grep-command-in-linux)[](#1-what-is-the-grep-command-in-linux)

The `grep` (Global Regular Expression Print) command is a powerful text-searching utility in Linux/Unix that allows users to search for specific patterns in files or output streams. It supports basic and extended regular expressions to match complex text patterns efficiently.

For a more developer-friendly alternative, you can check out [How to Install and Use Ack: A Grep Replacement for Developers on Ubuntu](/community/tutorials/how-to-install-and-use-ack-a-grep-replacement-for-developers-on-ubuntu-14-04) and [Awk command in Linux](/community/tutorials/awk-command-linux-unix#programming-concepts-for-awk-command).

### [2\. How do you find text using `grep` in Linux?](#2-how-do-you-find-text-using-grep-in-linux)[](#2-how-do-you-find-text-using-grep-in-linux)

To search for a word or phrase in a file, use:

```
grep "pattern" filename
```

For recursive searching in directories, use:

```
grep -r "pattern" directory/
```

To enhance searching with regular expressions, refer to [Using Grep Regular Expressions to Search for Text Patterns in Linux](/community/tutorials/using-grep-regular-expressions-to-search-for-text-patterns-in-linux).

### [3\. How do I use `grep` in Linux?](#3-how-do-i-use-grep-in-linux)[](#3-how-do-i-use-grep-in-linux)

Basic usage includes:

```
grep "text" file.txt
```

To ignore case sensitivity:

```
grep -i "text" file.txt
```

For advanced examples, you can check out this tutorial on [Top 50+ Linux Commands You MUST Know](/community/tutorials/linux-commands).

### [4\. How do I search all files in grep?](#4-how-do-i-search-all-files-in-grep)[](#4-how-do-i-search-all-files-in-grep)

To search for a pattern in all files within a directory:

```
grep -r "pattern" /path/to/directory
```

To include hidden files:

```
grep -r --exclude-dir=".*" "pattern" /path/to/directory
```

Be cautious with recursive searches, as large directories can slow down the process.

### [5\. How do I use grep to search recursively?](#5-how-do-i-use-grep-to-search-recursively)[](#5-how-do-i-use-grep-to-search-recursively)

To search for a pattern across multiple files in subdirectories, use the `-r` flag:

```
grep -r "search_term" /directory
```

For better control, use the `--include` or `--exclude` options:

```
grep -r --include="*.log" "error" /var/logs/
```

One common pitfall with recursive searches is encountering symbolic links, which can be handled using the `-R` option instead of `-r`.

### [6\. Comparison of `grep` and `awk` Commands in Linux](#6-comparison-of-grep-and-awk-commands-in-linux)[](#6-comparison-of-grep-and-awk-commands-in-linux)

Feature

`grep`

`awk`

Primary Function

Search for patterns in text

Text processing and manipulation

Pattern Matching

Supports basic and extended regular expressions

Supports regular expressions and more advanced pattern matching

Output Control

Limited output control

Full control over output format and content

Data Manipulation

No data manipulation capabilities

Can perform arithmetic, string manipulation, and conditional statements

Complexity

Simple and easy to use

More complex and powerful, but steeper learning curve

Typical Use Cases

Quick searches, filtering text

Data extraction, transformation, and reporting

`grep` is mainly used for searching patterns in text, whereas `awk` is more powerful for text processing and can manipulate data based on conditions.

Using `grep`:

```
grep "error" logfile.txt
```

Using `awk`:

```
awk '/error/ {print $2, $3}' logfile.txt
```

### [7\. How do I use regular expressions in `grep`?](#7-how-do-i-use-regular-expressions-in-grep)[](#7-how-do-i-use-regular-expressions-in-grep)

`grep` supports regex for advanced searching:

```
grep -E "error|fail|warning" logfile.txt
```

Example:

-   `^text` - Matches lines starting with “text”.
-   `text---
title: "Mastering Grep command in LinuxUnix A Beginners Tutorial"
date: 2025-02-14T00:00:00.000Z
draft: false
type: posts
categories: 
- unix-linux
---
# Mastering Grep command in LinuxUnix A Beginners Tutorial

<br/>

<br/>
 - Matches lines ending with “text”.
-   `[0-9]` - Matches any digit.

For a detailed guide, refer to out tutorial on using [Grep Regular Expressions to Search for Text Patterns in Linux](/community/tutorials/using-grep-regular-expressions-to-search-for-text-patterns-in-linux).

### [8\. Can `grep` be used in shell scripts?](#8-can-grep-be-used-in-shell-scripts)[](#8-can-grep-be-used-in-shell-scripts)

Yes! `grep` is widely used in shell scripts for filtering output.

```
if grep -q "error" logfile.txt; then
    echo "Errors found in the log."
fi
```

For more automation tips, check out this tutorial on [Top 50 Linux commands](/community/tutorials/linux-commands).

[Conclusion](#conclusion)[](#conclusion)
----------------------------------------

The `grep` command is an essential tool for searching text patterns in Linux and Unix systems. Whether you are filtering log files, searching for specific words in code repositories, or processing large datasets, grep provides powerful options like regular expressions, recursive searching, and multi-threaded execution to make your searches more efficient.

It is a must-have tool for system administrators, developers, and DevOps engineers. Keep experimenting with different flags and combinations to unlock its full potential.

Check out these related tutorials:

1.  [Using Grep Regular Expressions to Search for Text Patterns in Linux](/community/tutorials/using-grep-regular-expressions-to-search-for-text-patterns-in-linux)
2.  [How to Install and Use Ack: A Grep Replacement for Developers on Ubuntu](/community/tutorials/how-to-install-and-use-ack-a-grep-replacement-for-developers-on-ubuntu-14-04)
3.  [Top 50+ Linux Commands You MUST Know](/community/tutorials/linux-commands)

#### [Source](https://www.digitalocean.com/community/tutorials/grep-command-in-linux-unix)

<br/>
---

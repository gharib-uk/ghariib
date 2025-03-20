---
title: "Java Read File Complete Guide with Examples"
date: 2025-02-20T00:00:00.000Z
draft: false
type: posts
categories: 
- java
---
# Java Read File Complete Guide with Examples

<br/>

<br/>
### [Introduction](#introduction)[](#introduction)

In this article, you will learn about different ways to use Java to read the contents of a file line-by-line. This article uses methods from the following Java classes: `java.io.BufferedReader`, `java.util.Scanner`, `Files.readAllLines()`, and `java.io.RandomAccessFile`.

[Reading a File Line-by-Line using `BufferedReader`](#reading-a-file-line-by-line-using-bufferedreader)[](#reading-a-file-line-by-line-using-bufferedreader)
------------------------------------------------------------------------------------------------------------------------------------------------------------

You can use the `readLine()` method from `java.io.BufferedReader` to read a file line-by-line to String. This method returns `null` when the end of the file is reached.

Here is an example program to read a file line-by-line with `BufferedReader`:

ReadFileLineByLineUsingBufferedReader.java

```
package com.journaldev.readfileslinebyline;

import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class ReadFileLineByLineUsingBufferedReader {

 public static void main(String[] args) {
  BufferedReader reader;

  try {
   reader = new BufferedReader(new FileReader("sample.txt"));
   String line = reader.readLine();

   while (line != null) {
    System.out.println(line);
    // read next line
    line = reader.readLine();
   }

   reader.close();
  } catch (IOException e) {
   e.printStackTrace();
  }
 }

}
```

Continue your learning with the [`BufferedReader` API Doc](https://docs.oracle.com/javase/8/docs/api/java/io/BufferedReader.html#readLine--) (Java SE 8).

[Reading a File Line-by-Line using `Scanner`](#reading-a-file-line-by-line-using-scanner)[](#reading-a-file-line-by-line-using-scanner)
---------------------------------------------------------------------------------------------------------------------------------------

You can use the [`Scanner`](/community/tutorials/scanner-class-in-java) class to open a file and then read its content line-by-line.

Here is an example program to read a file line-by-line with `Scanner`:

ReadFileLineByLineUsingScanner.java

```
package com.journaldev.readfileslinebyline;

import java.io.File;
import java.io.FileNotFoundException;
import java.util.Scanner;

public class ReadFileLineByLineUsingScanner {

 public static void main(String[] args) {
  try {
   Scanner scanner = new Scanner(new File("sample.txt"));

   while (scanner.hasNextLine()) {
    System.out.println(scanner.nextLine());
   }

   scanner.close();
  } catch (FileNotFoundException e) {
   e.printStackTrace();
  }
 }

}
```

Continue your learning with the [`Scanner` API Doc](https://docs.oracle.com/javase/8/docs/api/java/util/Scanner.html) (Java SE 8).

[Reading a File Line-by-Line using `Files`](#reading-a-file-line-by-line-using-files)[](#reading-a-file-line-by-line-using-files)
---------------------------------------------------------------------------------------------------------------------------------

[`java.nio.file.Files`](/community/tutorials/java-files-nio-files-class) is a utility class that contains various useful methods. The `readAllLines()` method can be used to read all the file lines into a [list](/community/tutorials/java-list) of strings.

Here is an example program to read a file line-by-line with `Files`:

ReadFileLineByLineUsingFiles.java

```
package com.journaldev.readfileslinebyline;

import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Paths;
import java.util.List;

public class ReadFileLineByLineUsingFiles {

 public static void main(String[] args) {
  try {
   List<String> allLines = Files.readAllLines(Paths.get("sample.txt"));

   for (String line : allLines) {
    System.out.println(line);
   }
  } catch (IOException e) {
   e.printStackTrace();
  }
 }

}
```

Continue your learning with the [`Files` API Doc](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#readAllLines-java.nio.file.Path-) (Java SE 8).

[Reading a File Line-by-Line using `RandomAccessFile`](#reading-a-file-line-by-line-using-randomaccessfile)[](#reading-a-file-line-by-line-using-randomaccessfile)
------------------------------------------------------------------------------------------------------------------------------------------------------------------

You can use `RandomAccessFile` to open a file in _read mode_ and then use its `readLine` method to read a file line-by-line.

Here is an example program to read a file line-by-line with `RandomAccessFile`:

ReadFileLineByLineUsingRandomAccessFile.java

```
package com.journaldev.readfileslinebyline;

import java.io.IOException;
import java.io.RandomAccessFile;

public class ReadFileLineByLineUsingRandomAccessFile {

 public static void main(String[] args) {
  try {
   RandomAccessFile file = new RandomAccessFile("sample.txt", "r");
   String str;

   while ((str = file.readLine()) != null) {
    System.out.println(str);
   }

   file.close();
  } catch (IOException e) {
   e.printStackTrace();
  }
 }

}
```

Continue your learning with the [`RandomAccessFile` API Doc](https://docs.oracle.com/javase/8/docs/api/java/io/RandomAccessFile.html) (Java SE 8).

[Handling Different Encodings in Java File Reading](#handling-different-encodings-in-java-file-reading)[](#handling-different-encodings-in-java-file-reading)
-------------------------------------------------------------------------------------------------------------------------------------------------------------

If a file is stored in an encoding other than UTF-8, you should specify the correct encoding when reading it.

### [Reading a File with `UTF-8` Encoding](#reading-a-file-with-utf-8-encoding)[](#reading-a-file-with-utf-8-encoding)

```
import java.nio.charset.StandardCharsets;
import java.nio.file.*;

public class ReadUTF8File {
    public static void main(String[] args) {
        try {
            // This line reads the content of the file "example.txt" assuming it's encoded in UTF-8.
            // The Path.of("example.txt") method creates a Path object representing the file path.
            // The StandardCharsets.UTF_8 parameter specifies the character set to use for decoding.
            String content = Files.readString(Path.of("example.txt"), StandardCharsets.UTF_8);
            // This line prints the content of the file to the console.
            System.out.println(content);
        } catch (IOException e) {
            // This block catches any IOException that might occur during file reading and prints the stack trace.
            e.printStackTrace();
        }
    }
}
```

### [Reading a File with UTF-16 Encoding](#reading-a-file-with-utf-16-encoding)[](#reading-a-file-with-utf-16-encoding)

```
import java.nio.charset.Charset;
import java.nio.file.*;

public class ReadUTF16File {
    public static void main(String[] args) {
        try {
            // This line reads the content of the file "example.txt" assuming it's encoded in UTF-16.
            // The Path.of("example.txt") method creates a Path object representing the file path.
            // The Charset.forName("UTF-16") parameter specifies the character set to use for decoding.
            String content = Files.readString(Path.of("example.txt"), Charset.forName("UTF-16"));
            // This line prints the content of the file to the console.
            System.out.println(content);
        } catch (IOException e) {
            // This block catches any IOException that might occur during file reading and prints the stack trace.
            e.printStackTrace();
        }
    }
}
```

[Efficiently Handling Large Files Using Streams or `FileChannel`](#efficiently-handling-large-files-using-streams-or-filechannel)[](#efficiently-handling-large-files-using-streams-or-filechannel)
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

For massive files (GB-sized logs or datasets), Java’s NIO API (`FileChannel`) is a high-performance alternative to standard file reading.

Here is an example:

```
import java.io.IOException;
import java.nio.ByteBuffer;
import java.nio.channels.FileChannel;
import java.nio.file.*;

public class FileChannelExample {
    public static void main(String[] args) {
        try (FileChannel fileChannel = FileChannel.open(Path.of("largefile.txt"), StandardOpenOption.READ)) {
            // Allocate a ByteBuffer with a capacity of 4096 bytes to read data from the file channel.
            ByteBuffer buffer = ByteBuffer.allocate(4096);
            // Continuously read data from the file channel into the buffer until there is no more data to read.
            while (fileChannel.read(buffer) > 0) {
                // Flip the buffer to prepare it for reading. This sets the limit to the current position and the position to 0.
                buffer.flip();
                // Convert the buffer's content to a string and print it to the console. The parameters specify the start index, end index, and the character set.
                System.out.print(new String(buffer.array(), 0, buffer.limit()));
                // Clear the buffer to prepare it for the next read operation. This sets the position to 0 and the limit to the capacity.
                buffer.clear();
            }
        } catch (IOException e) {
            // Catch any IOException that might occur during file reading and print the stack trace.
            e.printStackTrace();
        }
    }
}
```

Using `FileChannel` significantly reduces memory usage compared to loading a file into memory at once.

For more advanced file handling techniques, you can check out this tutorial on [Java Files - java.nio.file.Files Class](/community/tutorials/java-files-nio-files-class).

[FAQs](#faqs)[](#faqs)
----------------------

### [1\. How to read a file in Java using `FileReader`?](#1-how-to-read-a-file-in-java-using-filereader)[](#1-how-to-read-a-file-in-java-using-filereader)

To read a file in Java using `FileReader`, you can create an instance of `FileReader` and read character data from the file. However, `FileReader` is not the most efficient option as it does not buffer the input. A better alternative is to wrap it inside a `BufferedReader`.

```
import java.io.FileReader;
import java.io.IOException;

public class FileReaderExample {
    public static void main(String[] args) {
        try (FileReader reader = new FileReader("example.txt")) {
            int data;
            while ((data = reader.read()) != -1) {
                System.out.print((char) data);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

For more efficient file reading, consider using [BufferedReader](/community/tutorials/java-read-text-file#java-read-text-file-using-java-io-bufferedreader) instead.

### [2\. How to read a file line by line in Java?](#2-how-to-read-a-file-line-by-line-in-java)[](#2-how-to-read-a-file-line-by-line-in-java)

The most common way to read a file line by line in Java is by using `BufferedReader`. This method is memory-efficient and performs well for large files.

Example using `BufferedReader`:

```
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class ReadFileLineByLine {
    public static void main(String[] args) {
        try (BufferedReader reader = new BufferedReader(new FileReader("example.txt"))) {
            String line;
            while ((line = reader.readLine()) != null) {
                System.out.println(line);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

For additional file handling methods, check out this tutorial on [Java File Handling](/community/tutorials/java-open-file).

### [3\. What is the most efficient way to read a large file in Java?](#3-what-is-the-most-efficient-way-to-read-a-large-file-in-java)[](#3-what-is-the-most-efficient-way-to-read-a-large-file-in-java)

When dealing with large files, reading the entire file into memory is inefficient. Instead, use streams or Java NIO (Non-blocking I/O) APIs like `FileChannel` for better performance.

#### Using BufferedReader with Streams (Efficient for Large Files)

```
import java.io.*;
import java.nio.file.*;

public class LargeFileReader {
    public static void main(String[] args) {
        try (BufferedReader reader = Files.newBufferedReader(Path.of("largefile.txt"))) {
            reader.lines().forEach(System.out::println);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

#### Using `FileChannel` (Best for Extremely Large Files)

For extremely large files, consider memory-mapped files or `FileChannel` for improved performance as described in the section above.

### [4\. How to handle file reading errors in Java?](#4-how-to-handle-file-reading-errors-in-java)[](#4-how-to-handle-file-reading-errors-in-java)

Error handling is crucial to avoid crashes due to **file not found, permissions issues, or encoding mismatches**.

-   Use try-with-resources to automatically close file resources.
-   Check file existence using `Files.exists(Path.of("file.txt"))`.
-   Catch exceptions like `IOException` to handle errors gracefully.

```
import java.io.*;

public class FileErrorHandling {
    public static void main(String[] args) {
        File file = new File("nonexistent.txt");

        if (!file.exists()) {
            System.out.println("File does not exist!");
            return;
        }

        try (BufferedReader reader = new BufferedReader(new FileReader(file))) {
            System.out.println(reader.readLine());
        } catch (IOException e) {
            System.out.println("Error reading file: " + e.getMessage());
        }
    }
}
```

### [5\. What is the difference between `FileReader` and `BufferedReader`?](#5-what-is-the-difference-between-filereader-and-bufferedreader)[](#5-what-is-the-difference-between-filereader-and-bufferedreader)

Feature

FileReader

BufferedReader

Performance

Reads one character at a time

Reads a full buffer (default 8192 bytes) at a time

Use case

Small files, simple character-based reading

Efficient reading, best for large files

Efficiency

Less efficient

Highly efficient

To handle large text files efficiently, always prefer `BufferedReader` over `FileReader`. Learn more about efficient file operations in tutorial on [Java read text file](/community/tutorials/java-read-text-file#java-read-text-file-using-java-io-bufferedreader).

[Conclusion](#conclusion)[](#conclusion)
----------------------------------------

In this comprehensive guide, you learned various methods to read the contents of a file line-by-line in Java, including the use of `BufferedReader`, `Scanner`, `Files.readAllLines()`, and `RandomAccessFile`. You also learned how to handle file reading errors, the difference between `FileReader` and `BufferedReader`, and how to efficiently handle large files using `FileChannel` or memory-mapped files.

Continue your learning with more [Java tutorials](/community/tags/java).

You can also refer to these tutorials on:

-   [Java Read Text File](/community/tutorials/java-read-text-file).
-   [Java Files - java.nio.file.Files Class](/community/tutorials/java-files-nio-files-class).
-   [Java Open File](/community/tutorials/java-open-file).

#### [Source](https://www.digitalocean.com/community/tutorials/java-read-file-line-by-line)

<br/>
---

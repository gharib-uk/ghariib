---
title: "<div>Modernizing a simple C++ application to Java with GitLab Duo</div>"
date: 2025-01-03
categories: 
  - "general"
tags: 
  - "development"
  - "git"
  - "github"
  - "gitlab"
  - "software"
---

Memory unsafe languages are those that do not handle any memory management on behalf of the developer. For example, when programming in C or C++, if you need memory during runtime, you will need to allocate and deallocate the memory yourself, running the risk of ending up with memory leaks in cases when you inadvertently forget to deallocate it. Other languages like Ada and FORTRAN provide some memory management but may not prevent memory leaks. Many organizations, including those in the public sector, have applications that have been developed using languages that are memory unsafe and are often looking to modernize these to a memory safe language, such as Java, Python, JavaScript, or Golang.

This tutorial focuses on a specific example of modernizing a simple C++ application to Java by refactoring it with the help of GitLab Duo, our suite of AI capabilities, and shows how much time and effort you can save in the migration.

## Understanding the simple C++ application

Let’s make the assumption that we have been tasked with the migration of a C++ application to a memory safe language, namely Java. The C++ application can be found in the following project (thank you to @sugaroverflow for contributing this sample application):

https://gitlab.com/gitlab-da/use-cases/ai/ai-applications/refactor-to-java/air-quality-application

Since this is the first time we are seeing this application, let’s invoke GitLab Duo Code explanation to better understand what it does. We open file `main.cpp` in Visual Studio Code and select the entirety of this file. We then right-click and select **GitLab Duo Chat > Explain selected snippet** from the popup menu.

![duo-code-explanation-menu-option](https://images.ctfassets.net/r9o86ar0p03f/18vrGTIjMuOByYSRIowhr0/bd2bd8a31cdadf73757f265714bdf3dc/code-explanation-menu-option.png)

The GitLab Duo Chat window opens up and the slash command `/explain` is executed for the selected code. Chat returns a very thorough and detailed description and explanation in natural language form of what each function does in the file as well as examples on how to run the compiled program.

![code-explanation-text](https://images.ctfassets.net/r9o86ar0p03f/7t53q0zQ4PYKoOMcexLUx7/08dfe137fe2fdbbec868872fe34df78a/code-explanation-text.png)

In short, the simple C++ application takes a U.S. zip code as input and returns the air quality index for that zip code.

## Compiling and running the C++ application

To further understand this simple C++ application, we proceed to compile and run it. We could have asked Chat how to do this, however, the project has a README file that provides the commands to compile the project, so we go ahead and use those by entering them in the Terminal window of VS Code.

![compile-command](https://images.ctfassets.net/r9o86ar0p03f/jnS8QMd3YaJEsU8v49G4w/25065bd3bce9eec30b7a1a50301836fd/compile-command.png)

After the compilation finishes, we change directory to the `build` subdirectory in the project, which is where the compilation process places the executable file for this application. Then, we run the executable by entering the following command:

`./air_quality_app 32836`

And we see the response as follows:

`Air Quality Index (AQI) for Zip Code 32836: 2 (Fair)`

![cplus-plus-app-execution-output](https://images.ctfassets.net/r9o86ar0p03f/4KZ2WtEGmXvj0QudG0X60p/e3a1fe946e8a79bbca771c451a0c5f71/cplus-plus-app-execution-output.png)

This confirms to us that the application was successfully compiled and it’s executing appropriately.

## Refactoring the application to Java

Let’s start migrating this C++ application to Java. We take advantage of GitLab Duo Chat and its refactoring capabilities by using the slack command `/refactor`. We qualify the slash command with specific instructions on what to do for the refactoring. We enter the following command in the Chat input field:

> /refactor this entire application to Java. Provide its associated pom.xml to build and run the Java application. Also, provide the directory structure showing where all the resulting files should reside for the Java application.

![refactor-chat-output](https://images.ctfassets.net/r9o86ar0p03f/dJ0RJ9oY0MFfo4I991fOF/652e507160fefd7f01b3d9e3907ca29a/refactor-chat-output.png)

Chat returns a set of Java files that basically refactor the entire C++ application to the memory safe language. In addition and per the prompt, Chat returns the pom.xml file, needed by maven for the building and execution of the refactored application as well as its directory structure, indicating where each generated file should reside.

We copy and save all the generated files to our local directory.

## Creating the Java project

In VS Code, we now proceed to open an empty project in which we will set up the directory structure of the new Java application and its contents.

We create all the previously generated Java files in their corresponding directories in the new project and paste their contents in each.

![java-files-created](https://images.ctfassets.net/r9o86ar0p03f/To1yAXGM0aHvfqCTHwNpa/47e93379eca044fd7cf1761ce23c761a/java-files-created.png)

Lastly, we save all the files to our local disk.

## Asking for help to build and run the Java application

At this point, we have an entire Java application that has been refactored from C++. Now, we need to build it but we don’t quite remember what maven command we need to use to accomplish this.

So we ask GitLab Duo Chat about this. We enter the following prompt in the Chat input field:

> How do you build and run this application using maven?

![maven-info-output](https://images.ctfassets.net/r9o86ar0p03f/2s87d5LTF4GM3wIjQqq4uT/cc40607673c2f59b3eda89d9f201e924/maven-info-output.png)

Chat returns with a thorough explanation on how to do this, including examples of the maven command to build and run the newly created Java application.

## Building and running the Java application

GitLab Duo Chat understands the application and environment context and responds that we first need to create an environment variable called `API_KEY` before we can run the application.

It also provides the maven command to execute to build the application, which we enter in the Terminal window:

```unset
mvn clean package
```

![java-build-output](https://images.ctfassets.net/r9o86ar0p03f/6sjDplbMvWylBq4ATMCaT4/adc24bb1197269748485e6d7bda3edfa/java-build-output.png)

Once the build finishes successfully, we copy the generated command to run the application from the Chat window and paste it in the Terminal window:

```unset
java -jar target/air-quality-checker-1.0-SNAPSHOT-jar-with-dependencies.jar 90210
```

![java-app-execution-output](https://images.ctfassets.net/r9o86ar0p03f/29wcsmJwpLS7t9EyMoEjqU/6c08a56ccc077d31eb09bc657cf2b611/java-app-execution-output.png)

The application successfully executes and returns the string:

```unset
Air Quality Index (AQI) for Zip Code 90210: 2 (Fair)
```

We have confirmed that the modernized version of the application, now refactored in Java, runs just like its original C++ version.

## Watch this tutorial in action

We have seen that by leveraging the power of GitLab Duo in your modernization activities, you can save a great deal of time and effort, freeing you to spend more time innovating and creating value to your organization.

Here is a video to show you, in action, the tutorial you just read:

<!-- blank line --> <figure class="video\_container"> <iframe src="https://www.youtube.com/embed/LJ7GOr\_P0xs?si=\_ZjF75DAXEQnY2Mn" frameborder="0" allowfullscreen="true"> </iframe> </figure> <!-- blank line -->

> #### Want to get started with GitLab Duo? Start a free, 60-day trial today!

## Learn more

- Refactor code into modern languages with AI-powered GitLab Duo
- Secure by Design principles meet DevSecOps innovation in GitLab 17
- How to secure memory-safe vs. manually managed languages

Memory unsafe languages are those that do not handle any memory management on behalf of the developer. For example, when programming in C or C++, if you need memory during runtime, you will need to allocate and deallocate the memory yourself, running the risk of ending up with memory leaks in cases when you inadvertently forget to deallocate it. Other languages like Ada and FORTRAN provide some memory management but may not prevent memory leaks. Many organizations, including those in the public sector, have applications that have been developed using languages that are memory unsafe and are often looking to modernize these to a memory safe language, such as Java, Python, JavaScript, or Golang.

This tutorial focuses on a specific example of modernizing a simple C++ application to Java by refactoring it with the help of GitLab Duo, our suite of AI capabilities, and shows how much time and effort you can save in the migration.

## Understanding the simple C++ application

Let’s make the assumption that we have been tasked with the migration of a C++ application to a memory safe language, namely Java. The C++ application can be found in the following project (thank you to @sugaroverflow for contributing this sample application):

https://gitlab.com/gitlab-da/use-cases/ai/ai-applications/refactor-to-java/air-quality-application

Since this is the first time we are seeing this application, let’s invoke GitLab Duo Code explanation to better understand what it does. We open file `main.cpp` in Visual Studio Code and select the entirety of this file. We then right-click and select **GitLab Duo Chat > Explain selected snippet** from the popup menu.

![duo-code-explanation-menu-option](https://images.ctfassets.net/r9o86ar0p03f/18vrGTIjMuOByYSRIowhr0/bd2bd8a31cdadf73757f265714bdf3dc/code-explanation-menu-option.png)

The GitLab Duo Chat window opens up and the slash command `/explain` is executed for the selected code. Chat returns a very thorough and detailed description and explanation in natural language form of what each function does in the file as well as examples on how to run the compiled program.

![code-explanation-text](https://images.ctfassets.net/r9o86ar0p03f/7t53q0zQ4PYKoOMcexLUx7/08dfe137fe2fdbbec868872fe34df78a/code-explanation-text.png)

In short, the simple C++ application takes a U.S. zip code as input and returns the air quality index for that zip code.

## Compiling and running the C++ application

To further understand this simple C++ application, we proceed to compile and run it. We could have asked Chat how to do this, however, the project has a README file that provides the commands to compile the project, so we go ahead and use those by entering them in the Terminal window of VS Code.

![compile-command](https://images.ctfassets.net/r9o86ar0p03f/jnS8QMd3YaJEsU8v49G4w/25065bd3bce9eec30b7a1a50301836fd/compile-command.png)

After the compilation finishes, we change directory to the `build` subdirectory in the project, which is where the compilation process places the executable file for this application. Then, we run the executable by entering the following command:

`./air_quality_app 32836`

And we see the response as follows:

`Air Quality Index (AQI) for Zip Code 32836: 2 (Fair)`

![cplus-plus-app-execution-output](https://images.ctfassets.net/r9o86ar0p03f/4KZ2WtEGmXvj0QudG0X60p/e3a1fe946e8a79bbca771c451a0c5f71/cplus-plus-app-execution-output.png)

This confirms to us that the application was successfully compiled and it’s executing appropriately.

## Refactoring the application to Java

Let’s start migrating this C++ application to Java. We take advantage of GitLab Duo Chat and its refactoring capabilities by using the slack command `/refactor`. We qualify the slash command with specific instructions on what to do for the refactoring. We enter the following command in the Chat input field:

> /refactor this entire application to Java. Provide its associated pom.xml to build and run the Java application. Also, provide the directory structure showing where all the resulting files should reside for the Java application.

![refactor-chat-output](https://images.ctfassets.net/r9o86ar0p03f/dJ0RJ9oY0MFfo4I991fOF/652e507160fefd7f01b3d9e3907ca29a/refactor-chat-output.png)

Chat returns a set of Java files that basically refactor the entire C++ application to the memory safe language. In addition and per the prompt, Chat returns the pom.xml file, needed by maven for the building and execution of the refactored application as well as its directory structure, indicating where each generated file should reside.

We copy and save all the generated files to our local directory.

## Creating the Java project

In VS Code, we now proceed to open an empty project in which we will set up the directory structure of the new Java application and its contents.

We create all the previously generated Java files in their corresponding directories in the new project and paste their contents in each.

![java-files-created](https://images.ctfassets.net/r9o86ar0p03f/To1yAXGM0aHvfqCTHwNpa/47e93379eca044fd7cf1761ce23c761a/java-files-created.png)

Lastly, we save all the files to our local disk.

## Asking for help to build and run the Java application

At this point, we have an entire Java application that has been refactored from C++. Now, we need to build it but we don’t quite remember what maven command we need to use to accomplish this.

So we ask GitLab Duo Chat about this. We enter the following prompt in the Chat input field:

> How do you build and run this application using maven?

![maven-info-output](https://images.ctfassets.net/r9o86ar0p03f/2s87d5LTF4GM3wIjQqq4uT/cc40607673c2f59b3eda89d9f201e924/maven-info-output.png)

Chat returns with a thorough explanation on how to do this, including examples of the maven command to build and run the newly created Java application.

## Building and running the Java application

GitLab Duo Chat understands the application and environment context and responds that we first need to create an environment variable called `API_KEY` before we can run the application.

It also provides the maven command to execute to build the application, which we enter in the Terminal window:

```unset
mvn clean package
```

![java-build-output](https://images.ctfassets.net/r9o86ar0p03f/6sjDplbMvWylBq4ATMCaT4/adc24bb1197269748485e6d7bda3edfa/java-build-output.png)

Once the build finishes successfully, we copy the generated command to run the application from the Chat window and paste it in the Terminal window:

```unset
java -jar target/air-quality-checker-1.0-SNAPSHOT-jar-with-dependencies.jar 90210
```

![java-app-execution-output](https://images.ctfassets.net/r9o86ar0p03f/29wcsmJwpLS7t9EyMoEjqU/6c08a56ccc077d31eb09bc657cf2b611/java-app-execution-output.png)

The application successfully executes and returns the string:

```unset
Air Quality Index (AQI) for Zip Code 90210: 2 (Fair)
```

We have confirmed that the modernized version of the application, now refactored in Java, runs just like its original C++ version.

## Watch this tutorial in action

We have seen that by leveraging the power of GitLab Duo in your modernization activities, you can save a great deal of time and effort, freeing you to spend more time innovating and creating value to your organization.

Here is a video to show you, in action, the tutorial you just read:

<!-- blank line --> <figure class="video\_container"> <iframe src="https://www.youtube.com/embed/LJ7GOr\_P0xs?si=\_ZjF75DAXEQnY2Mn" frameborder="0" allowfullscreen="true"> </iframe> </figure> <!-- blank line -->

> #### Want to get started with GitLab Duo? Start a free, 60-day trial today!

## Learn more

- Refactor code into modern languages with AI-powered GitLab Duo
- Secure by Design principles meet DevSecOps innovation in GitLab 17
- How to secure memory-safe vs. manually managed languages

Go to Source

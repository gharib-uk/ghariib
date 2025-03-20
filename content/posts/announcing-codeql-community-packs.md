---
title: "Announcing CodeQL Community Packs"
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

We are excited to introduce the new CodeQL Community Packs, a comprehensive set of queries and models designed to enhance your code analysis capabilities. These packs are tailored to augment…

The post Announcing CodeQL Community Packs appeared first on The GitHub Blog.

We are excited to introduce the new CodeQL Community Packs, a comprehensive set of queries and models designed to enhance your code analysis capabilities. These packs are tailored to augment the standard set of CodeQL queries, providing additional resources for security researchers and developers alike.

## Why?

CodeQL is a semantic code analysis tool that allows developers to query their codebases as databases, enabling the identification of vulnerabilities, bugs, and patterns efficiently.

The standard set of CodeQL queries is focused on accuracy and low false positive rates, which is ideal for integration into CI/CD pipelines where alerts are primarily handled by developers. However, when alerts are operated by security engineers or researchers, the balance between false positives and false negatives can be adjusted to prioritize low false negatives, ensuring no bugs are left behind—albeit at the cost of more triaging.

## What?

The CodeQL Community Packs is a set of CodeQL packs to augment the standard set queries. They include three main types of packs:

- **Model packs**: these packs contain additional models of Taint Tracking sources, sinks, and summaries for libraries and frameworks that are not supported by the default suites.
- **Query packs**: these packs contain additional security and audit queries to help identify potential vulnerabilities and improve code quality.
- **Library packs**: designed to be used by query packs, these packs do not contain queries themselves but provide essential libraries for more comprehensive analysis.

## How?

The GitHub Security Lab has been extensively using these packs for the last few years and as our records show, they turned out to be very fruitful.

![Badge that says 381 vulnerabilities found with the help of CodeQL.](https://github.blog/wp-content/uploads/2024/12/381-vulnerabilities.png?w=683&resize=683%2C529)

In addition to the additional queries and models provided by the community packs, we have also been using the audit queries, which proved invaluable when running deep-dive manual code reviews, such as the ones we did for Datahub and Home Assistant. Being able to list all the files which introduced untrusted data into the application or that perform security-relevant operations was really helpful when exploring unfamiliar huge codebases, such as the Home Assistant one.

![Screenshot of the section 'Analyzing the code base' from a blog post about Home Assistant.](https://github.blog/wp-content/uploads/2024/12/home-assistant-codebas.png?w=746&resize=746%2C198)

## What’s in the community packs?

The CodeQL Community Packs offer a variety of additional queries and models for languages, such as Java, C#, and Python. These packs are designed to move the Signal to Noise (SNR) ratio closer to the low false negatives end of the spectrum, making them particularly useful for security researchers.

For example, the Java packs include:

- Java queries
    - **CVEs**: queries for known CVEs such as Log4Shell.
    - **Security**: dozens of new security queries contributed by CodeQL engineers and security researchers from the GitHub Security Lab, but also by the broader community of security researchers.
    - **Audit Exploration**: queries to list all files, dependencies, untrusted data entry points, and hazardous sinks.
    - **Audit Templates**: templates to build your own taint tracking queries, explore data paths, or “hoist” sinks to public method parameters.
    - **Library sources**: special queries designed to find third-party APIs called with untrusted data.
- Java extension models
    - This pack contains additional models which define additional remote flow sources, summaries and sinks for hundreds of APIs.
- Java libraries
    - A collection of predicates and classes used by Java queries.
- Library extension models
    - Additional threat model pack which defines library API method parameters as a source of untrusted data.

![Screenshot of what is included in the Java CodeQL Community Pack](https://github.blog/wp-content/uploads/2024/12/file-tree.png?w=669&resize=669%2C1024)

## Library extension models

Remember Log4Shell? It was relatively easy for a SAST tool to detect, as the JNDI injection sink was well-known and covered by existing CodeQL models at that time. However, CodeQL’s default threat model, like most SAST tools, is based on modeling untrusted data as data that comes from the network. Therefore, CodeQL could have reported Log4Shell if we had analyzed an application that took untrusted data from the network (for example, a web application) and passed this untrusted data to Log4J logger methods.

To enable CodeQL to report such a data flow path, we would have needed to provide CodeQL with the source code of both the web application and Log4J. Could we have reported Log4Shell by analyzing only the Log4J source code? Certainly! But we would have needed a different threat model, one in which the arguments of logger methods such as `info` or `error` were considered sources of untrusted data. But how could CodeQL know that these methods could introduce untrusted data in the first place?

To support such a threat model, we developed the library source packs. We analyzed thousands of applications that took untrusted data and passed it to third-party APIs (such as Log4J’s `error` method). This analysis resulted in a list of third-party library methods used in real applications that are passed untrusted data.

Once we collected this list, which contained API methods such as Log4J’s `AbstractLogger.error`, we used it to define new sources of untrusted data to be used when scanning library code, such as Log4J code. By doing this with Log4J code, we were able to first identify that logger methods can be called with untrusted data from network requests and second, report a JNDI injection in Log4J code when using the new library source QL packs!

![Screenshot of an open issue titled 'JNDI-lookup with user-controlled name.' Beneath the title, there is a banner reading 'Speed up the remediation of this alert with Copilot Autofix for CodeQL.'](https://github.blog/wp-content/uploads/2024/12/kndi-lookup.png?w=1024&resize=1024%2C665)

## Exploration queries

Reviewing a new, unfamiliar codebase is a difficult and lengthy process. Reducing the review surface to the most significant and relevant files is crucial to making this process as efficient as possible.

When faced with similar reviews, the GitHub Security Lab likes to first map out the new codebase. We do this by listing all the entry points where potentially untrusted data enters the application and identifying operations that can be hazardous, such as file reads/writes, deserialization operations, or network requests.

To achieve this, we use the RemoteFlowSources.ql query, which provides a list of all places identified by CodeQL where untrusted data enters the application. We also use the HotSpots query, which returns a list of all hazardous sinks in the application, regardless of evidence of untrusted data flowing into them.

In addition to providing a good initial heat map of the codebase, this approach helps us better understand how well CodeQL covers the used libraries and whether additional modeling is needed.

### How to use them?

The community packs are regular CodeQL packs and can be used both as part of GitHub’s code scanning workflows and with the CodeQL CLI.

To use the CodeQL community packs in code scanning, specify a `with: packs:` entry in the `uses: github/codeql-action/init@v3` section of your CodeQL code scanning workflow. See the examples below.

Adding the community packs **library extension models** to a scan:

```
- name: Initialize CodeQL
        uses: github/codeql-action/init@v3
        with:
          languages: java
          packs: githubsecuritylab/codeql-java-library-sources,githubsecuritylab/codeql-java-extensions
```

Running the community packs **additional security queries**:

```
- name: Initialize CodeQL
        uses: github/codeql-action/init@v3
        with:
          languages: java          
          queries: java
          packs: githubsecuritylab/codeql-java-queries
```

Running the community packs **additional security queries** with the additional community packs **extension models**:

```
- name: Initialize CodeQL
        uses: github/codeql-action/init@v3
        with:
          languages: java          
          queries: java
          packs: githubsecuritylab/codeql-java-extensions,githubsecuritylab/codeql-java-queries
```

Similarly, you can use the community packs from the CLI.

Adding the community packs **library extension models** to a scan:

```
codeql database analyze --download <CodeQL DB> --model-packs githubsecuritylab/codeql-java-extensions --model-packs githubsecuritylab/codeql-java-library-sources codeql/java-queries --format=sarif-latest --output=scan.sarif --sarif-add-file-contents
```

Running the community packs additional **security queries**:

```
codeql database analyze --download <CodeQL DB> githubsecuritylab/codeql-java-queries --format=sarif-latest --output=scan.sarif --sarif-add-file-contents
```

Running the community packs additional **security queries** with the additional community packs **extension models**:

```
codeql database analyze --download db --model-packs githubsecuritylab/codeql-java-extensions githubsecuritylab/codeql-java-queries --format=sarif-latest --output=scan.sarif --sarif-add-file-contents
```

## How to contribute?

The most important aspect of the community packs is the community involvement! Sharing your models and queries with the community is the best way to help secure the open source software we all depend on. Contributions can range from simple Model As Data (MaD) lines to existing extension files or even the creation of new queries that model new vulnerability classes. Every contribution is welcome!

The post Announcing CodeQL Community Packs appeared first on The GitHub Blog.

Go to Source

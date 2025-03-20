---
title: "How to manage Python dependencies in Ansible execution environments"
date: 2025-01-29
---

Ansible execution environments provide a runtime for Red Hat Ansible Automation Platform that includes all of the necessary tooling in a single atomic package. Ansible, as an automation tool, relies on Python as its underlying programming language. Not only is Python integral to the core of Ansible, but its use is widespread and can be found throughout the Ansible ecosystem, including Ansible execution environments. As a result, how Python content is managed directly impacts how an execution environment is constructed and operates at runtime. This article will explore the various ways that Python can be managed in Ansible execution environments, including considerations that apply to a variety of operating environments.

## Python within execution environments

An Ansible execution environment is a container image, and its construction is expressed within a definition file (by default with the name `execution-environment.yml`) which enables operating system packages, Python content, Ansible dependencies, and other configurations to be specified. Multiple options are available within the execution environment definition to enable end users to manage how the base Python package is configured within the underlying operating system, but also how Python packages and any other dependencies are handled.

The dependencies section of the execution environment definition file contains several properties that users can customize the use of Python along with any related content. The underlying Python package that the execution environment relies on is defined within the `dependencies.python_interpreter` property. Two options are available within the `python_interpreter` property:

1. `package_system`: Python package managed by the operating system package manager (such as `dnf`).
2. `python_path`: Location of the Python interpreter.

An example of how these properties can be defined within an execution environment definition file is shown below:

```plaintext
---
version: 3

...

dependencies:
  python_interpreter:
    package_system: "python310"
    python_path: "/usr/bin/python3.10" 
```

## Python dependency management

The management of Python packages is facilitated using the pip package manager. `pip` takes on responsibility for managing core Ansible execution environment components, including `ansible-core` and `ansible-runner`, as well as dependencies declared by Ansible Content Collections along with any user-defined packages.

Customizations to the Ansible Core and Ansible Runner packages can be defined by specifying the following underneath the `dependencies` within the execution environment definition file:

```plaintext
---
version: 3

...

dependencies:
  ansible_core:
    package_pip: ansible-core==2.14.2
  ansible_runner:
    package_pip: ansible-runner==2.3.1
```

While users have the ability to customize how the underlying Python package and core Ansible components that are managed within an execution environment are defined, most end users will be concerned with how to specify additional Python packages that should also be included within an execution environment. These packages are used primarily by Ansible Content Collections that are also included within an execution environment, as are any other components that rely on Python content.

User-defined Python dependencies can either be explicitly defined within the execution environment definition file or via a reference to a file (typically specified within a file called `requirements.txt`) containing the dependencies to install. The `dependencies.python` property within the execution environment definition file is the location where either of these approaches are declared. 

The following example illustrates how to explicitly declare the desired Python dependencies within the execution environment definition file:

```plaintext
---
version: 3

...

dependencies:
  # Explicitly declaring Python dependencies
  python:
    - jmespath
```

Alternatively, dependencies declared within an external file can be referenced within an execution environment definition file, and can be specified using the following:

```plaintext
---
version: 3

...

dependencies:
  # Referencing an external file containing Python dependencies
  python: requirements.txt
```

Regardless of the approach selected, Ansible execution environments offer options for Ansible creators to choose the approach that suits them best.

## Build time considerations

Thus far, we have discussed several methods for how to define Python-related content that is included within Ansible execution environments. In this section, we will build upon those skills and explore how this Python content is managed during the build process of an execution environment, including the ways that you can influence the final image that is produced as a result of the build.

When an execution environment is being built, it undergoes several stages where the resulting content is used to assemble the final image. (You can learn more about the execution environment build stages and the actions the `ansible-builder` tool performs here). Several of these stages have activities directly related to how Python content is utilized and sourced from remote locations. The properties that were defined earlier, such as Python executable version and location, are utilized throughout this process. The `additional_build_steps` property of the execution environment definition file provides the constructs for users to add customized logic (in the form of commands that are processed by the container tool, such as Podman, in use) within the build process, and is where any of the activities related to the management of Python content can be made. The `builder` stage of the execution environment build process is primarily where Python-related activities occur including the resolution of declared dependencies and is where any customizations should take place. The `base` and `final` stages also play a role in how Python content is assembled into the final image.

As an Ansible creator, there are at least two primary use cases (though there are certainly others as well) for which there may be a need to interject in the default build of an execution environment:

1. Upgrading core `pip` components.
2. Sourcing of Python dependencies managed by `pip`.

Similar to how there is a need to ensure that Python and any of the libraries needed to support Ansible automation (as well as the underlying operating system) within an execution environment are updated, it is also important that `pip` and any of the tooling that it relies upon (such as setuptools) is up to date as well. Let’s show how we can verify that the latest version of both `pip` and `setuptools` are included within the execution environment.

For this task, we will add a new step just prior to when actions in the `base` stage are executed. The `additional_build_steps` property offers the `prepend_base` option to enable additional commands to accomplish this goal. See below:

```plaintext
---
version: 3
...

additional_build_steps:
  prepend_base:
    - RUN $PYCMD -m pip install --upgrade pip setuptools
```

`RUN` is an instruction of the container tool to execute commands and `$PYCMD` is one of several environment variables that are exposed throughout the build process by the `ansible-builder` tool. `$PYCMD` refers to the location of the Python binary, and as you can see, it is being used here to upgrade both `pip` and `setuptools` packages to their latest versions to provide a solid foundation for Python content to build upon.

With an assurance that the base Python content within an execution environment is updated, focus can then shift to the management of dependencies as declared by the user. By default, Python dependencies are sourced from Python Package Index (PyPi), a public hosted repository containing Python content. The ability to easily reference content from a public service is beneficial, as it simplifies how Python content is sourced. However, in many enterprise organizations, they establish and leverage their own internal repository (similar to PyPi) to govern the type of content that can be used. Access to the public PyPi repository is typically restricted or even forbidden. In these situations and environments, `ansible-builder` must be instructed to source content from this alternate location.

The `pip` utility provides options at package install time for referencing locations other than the public PyPi repository which contain Python packages. They can be explicitly set via command line arguments. However, similar to what we saw previously, it is easier to make use of environment variables, as it reduces the overall complexity for specifying different configurations.

The following environment variables can be set to influence how `pip` sources Python content:

- `PIP_INDEX_URL`: Location of the PEP 503 compliant repository (the default location is here).
- `PIP_TRUSTED_HOST`: Explicitly trust the host defined. Useful when operating in environments that leverage their own Certificate Authority (CA) for managing Secure Sockets Layer (SSL) certificates which may not be trusted by the default configuration of the base execution environment being used as the foundation for the new execution environment.

These variables can be placed as needed, and are leveraged within the builder and final stages of an execution environment build. To ensure they are defined before starting those stages, we can use the `prepend_builder` and `prepend_final` of the `additional_build_steps` property to inject those environment variables, as shown below:

```plaintext
---
version: 3
...

additional_build_steps:
  prepend_builder:
    - ENV PIP_INDEX_URL=https://myprivatrepohost/pythonrepo/simple
  prepend_final:
    - ENV PIP_INDEX_URL=https://myprivatrepohost/pythonrepo/simple
```

If authentication is required to access content from the repository, credentials can be provided (especially if utilizing basic authentication) as part of the URL using the form `https://<username>:<password>@myprivatrepohost/pythonrepo/simple` and replacing the `<username>` and `<password>` placeholders with their actual values.

Alternatively, instead of specifying credentials within the URL, support is available within `pip` to authenticate using alternate sources, including a `.netrc` file or a keyring. The configurations needed go beyond the scope of this article, but the inclusion here highlights the options that are available for accessing protected repositories.

Another enhancement that can be made is to make use of Dockerfile/Containerfile arguments using the `ARG` instruction instead of the `ENV` instruction. This offers the flexibility to reuse the same execution environment definition file among multiple environments where Python dependencies may be sourced from different locations (such as between development and production environments).

The following execution environment file depicts the changes necessary to transition from `ENV` to `ARG` instructions:

```plaintext
---
version: 3
...

additional_build_steps:
  prepend_builder:
    - ARG PIP_INDEX_URL
  prepend_final:
    - ARG PIP_INDEX_URL
```

At execution environment build time, values can be injected by specifying the `--build-arg` flag when invoking the `ansible-builder build` command. For example, to replicate the first example where the `ENV` instruction is being utilized, the following command can be used:

```plaintext
ansible-builder build --build-arg=PIP_INDEX_URL=https://myprivatrepohost/pythonrepo/simple <additional arguments>
```

A useful feature of the `ansible-builder` utility is that if the environment variable is already defined in the current shell, it will automatically be injected into the value of the build argument being specified (`--build-arg=PIP_INDEX_URL`).

## Conclusion

In this article, I described the various ways that Python-related content can be managed when building Ansible execution environments. From the ability to define the base Python components to the management of Python dependencies, these approaches afford Ansible creators the opportunity to customize and tune the build process such that Ansible execution environments can be utilized across a variety of operating environments.

Interested in learning more about building and utilizing Ansible execution environments in your own automation workflows? Explore both the associated documentation as well as interactive exercises to level up your skills below.

Documentation:

- Creating and Using Ansible Execution Environments
- Using Ansible Content Navigator

Interactive exercises:

- Getting started with Ansible Builder
- Getting started with Ansible Navigator

The post How to manage Python dependencies in Ansible execution environments appeared first on Red Hat Developer.  
  

Go to Source

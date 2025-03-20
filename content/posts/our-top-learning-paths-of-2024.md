---
title: "Our top learning paths of 2024"
date: 2025-01-02
categories: 
  - "general"
---

It can't be almost January yet. As the new year approaches, it's time to reflect on the top learning paths Red Hat Developer published this year, along with highlighting key paths that were too new to make the list.

No matter what kind of developer you are, it can be difficult enough to keep up with the technologies you already work with. Learning new areas, tools, and products can be daunting on top of managing heavy workloads, so structured self-service developer learning has come to the forefront as an important option for skilling up and learning new things. That explains the strong success of our beginner content, as you will see.

Below the top 10 learning paths of 2024, I have also included some of our newest learning paths on hot topics like artificial intelligence (AI) and virtualization.

**Catch up on the rest of our best of 2024 series:** Ansible automation, AI, Kubernetes and OpenShift, Linux, programming languages and runtimes

## The 10 most popular learning paths published in 2024

Many of our top learning paths use our Developer Sandbox for a hands-on component. The sandbox is a no-cost 30-day cluster that lets you bring your own code and experiment with a growing variety of Red Hat products and related technologies. 

### #10: Develop containers using Podman Desktop and Kubernetes

By **Don Schenck**

Building cloud-native applications can feel like an onerous process. In this learning path, you will learn how to go from an initial application to deploying it as a Kubernetes container onto Red Hat OpenShift via our no-cost Developer Sandbox. 

Along the way you will learn about the components that form a complete ecosystem for developing, packaging, and managing applications in an efficient, scalable manner: containers, images, (Kubernetes) pods, and Kubernetes itself. You will use the OpenShift command-line (CLI) tool `oc`, Podman and Podman Desktop, and container registries like Quay.io.

Podman Desktop is a lightweight and efficient tool for managing containers and working with Kubernetes from your Windows, macOS, or Linux machine. From building container images to working with registries and deploying containers, Podman Desktop helps you seamlessly work with containerization technology, and simplifies the transition to container deployment on Kubernetes. Together with the Developer Sandbox, it allows an iterative development approach for services.

The example application is written in Python, but go ahead and try it with your own code in the Sandbox once you're done.

Read it here: **Develop containers using Podman Desktop and Kubernetes**

### #9: Foundations of Ansible

By **Craig Brandt**

You might have heard of Ansible, a popular automation platform used by platform engineers and DevOps teams to create, manage, and scale automation across physical, cloud, virtual, and edge environments.

In this learning path, you will learn key Ansible concepts like playbooks, modules, tasks, and roles, along with control nodes and managed hosts. You will then set up common developer productivity tools and environments including the basics of using Git and an introduction to Visual Studio Code. Finally, you will explore Ansible development tools that assist the creation, testing, and deployment of your automation code.

Read it here: **Foundations of Ansible**

### #8: Get started with Ansible Playbooks

By **Craig Brandt**

Once you understand Ansible basics, you will probably want to try your hand at automating some of your own repetitive tasks. In this learning path, you will learn how to create an Ansible Playbook using best practices. It starts by explaining core Ansible Playbook concepts and progresses to more advanced topics like variables, conditionals, and loops.

What are playbooks written in? YAML, which takes us coincidentally to #7.

Read it here: **Get started with Ansible Playbooks**

### #7: YAML essentials for Ansible

By **Craig Brandt**

What is YAML? YAML is a simple yet powerful data serialization language that focuses on data markup instead of documents. If you have never used YAML before, don't worry. This learning path starts from the basics.

It begins with the advantages of using YAML for Ansible Playbooks and configurations. From there, you will learn YAML syntax and structure, including working with YAML files, lists, and directories. And, of course, how to add comments for better readability and maintenance.

While this path focuses on how Ansible uses YAML for playbooks, knowing this language is useful in other ways. For example, YAML is also used in Kubernetes and OpenShift configurations, along with Backstage and Red Hat Developer Hub. 

Read it here: **YAML essentials for Ansible**

### #6: Get started with the Ansible Visual Studio Code extension

By **Anshul Behl**

Many developers prefer to use an integrated development environment (IDE) when writing code, and a popular IDE is Visual Studio Code (VS Code). Using this IDE and the Ansible VS Code extension gives you syntax highlighting, auto-completion, linting, and integration with AI tools like Red Hat Ansible Lightspeed with IBM watsonx Code Assistant. 

By the end of this learning path you will know how to execute Ansible Playbooks directly from within VS Code and integrate version control.

Read it here: **Get started with the Ansible Visual Studio Code extension**

### #5: Install and configure Red Hat Developer Hub and explore templating basics

By **Ian Lawson**

You are probably well aware of the cognitive overload many development teams are facing these days. There are so many tools and technologies available that teams struggle to know what is approved, when and where to use specific languages and frameworks, efficiently onboard new team members, and so on. 

Add in the need to manage development infrastructure, secure software supply chains, make documentation easily accessible, and more, and you start to hear terms like internal developer portal (IDP) and platform engineering. What often follows is mentions of the open source Backstage.io, which is a framework to create internal developer portals. 

This learning path begins by introducing Backstage and its basics. Then it focuses on what Red Hat Developer Hub offers in addition: features like pre-installed visualization and cloud-native development plug-ins. Based on Backstage, Red Hat Developer Hub is an enterprise-grade, internal developer platform with supported plug-ins for integration with Red Hat technologies like Red Hat OpenShift.

After the basics, you will learn about key pre-installed plug-ins, how to send users to important documentation and other references from within Developer Hub, how to try it with OpenShift for no cost with our Developer Sandbox, integrate Developer Hub with GitHub, and how to create a template to provide developers with a code repository to work on and documentation to assist.

Read it here: **Install and configure Red Hat Developer Hub and explore templating basics**

### #4:  How to deploy a Java application on Kubernetes in minutes

By **Don Schenck**

Application modernization is a big topic, given the need to improve software delivery performance for legacy software systems. Completely rewriting a large application for the microservices era and cloud platforms is a daunting and potentially expensive task. Instead, it's often more efficient to update the monolithic build into smaller pieces that work on the cloud.

Instead of getting into that level of complexity, this learning path focuses on how to move your legacy Java application into a container and deploy it to Kubernetes. You will do this hands-on within the Developer Sandbox, ultimately deploying your containerized Java app to Red Hat OpenShift using only Kubernetes commands.

By the end of this learning path you will have deployed a MySQL database and a sample Java application from source code using Source-to-Image (S2I), both with configured environment variables. 

Read it here: **How to deploy a Java application on Kubernetes in minutes**

### #3: Learn Kubernetes using the Developer Sandbox

By **Don Schenck**

Before using Red Hat OpenShift for your application development platform, it helps to understand Kubernetes. You can learn both hands-on using our no-cost Developer Sandbox, since OpenShift is built on top of Kubernetes. This learning path uses only Kubernetes commands to show you how to create both a back-end and front-end application and connect the two, create a database application running in Kubernetes and populate it from the command line, scale an application with one command, and update an application on the fly. 

While the examples use Python 3.8, React, and MariaDB, once you're done you still have the Sandbox cluster for a total of 30 days. Try out your new skills using your own code and other available frameworks and tools next. Or, learn how to do the same thing in OpenShift instead to see the differences. See learning path #2.

Read it here: **Learn Kubernetes using the Developer Sandbox**

### #2: How to deploy full-stack JavaScript applications in OpenShift

By **Don Schenck**

Any development starts with gathering your tools, and it's no different for cloud-native applications. While there are more tools involved, they build on top of the tools you might already know and are familiar with. For example, you have your language tools for developing the initial application. You might use Git and GitHub for bringing in the sample code and for version control. 

On top of that, this learning path adds Podman (short for pod manager) for developing, managing, and running containers. You will also use an online container image registry like Quay.io. Because this path focuses on Red Hat OpenShift, you will also install the `oc` command-line tool and use the Developer Sandbox so you can work through the entire process hands-on.

This learning path walks you through taking the source code for an application running locally, and then deploying that application to Red Hat OpenShift in the Developer Sandbox. It starts by having you set up your working environment for Node.js and OpenShift development. Then you will run the application locally, build a container for the front end, and deploy it to OpenShift.

Next, you will deploy a database to OpenShift, then deploy the back end and connect it to the database and front end using environment variables. Finally, you will add a health check, deploy a new microservice from an existing image, and connect all of the services using environment variables.

All of these skills come together for a complete experience. While the example code uses JavaScript and Node.js, when you are finished you can turn around and use this learning path as a reference to do the same with your own code. 

Read it here: **How to deploy full-stack JavaScript applications in OpenShift**

### #1: Get started with your Developer Sandbox

By **Don Schenck**

I have talked a lot about the Developer Sandbox by this point. It's no surprise that our top new learning path is how to get started with it. After all, many developers want to experiment with cloud-native technologies without having to install and configure the complex tools involved. You might not even have the infrastructure to do so.

This learning path covers the basics of how to get into the Developer Sandbox and Red Hat OpenShift, change to the OpenShift Developer perspective, add an application to your cluster from source code, test your application, and build it from a container image.

While as of this writing, this learning path focuses on Red Hat OpenShift, the Sandbox also offers Red Hat OpenShift Dev Spaces, Red Hat OpenShift AI, and more. Once you're in there and comfortable, why not explore?

Read it here: **Get started with your Developer Sandbox**

## Other paths worth noting

Top 10 lists only show part of the whole. Here are additional learning paths on themes that have become popular this year. 

### AI learning paths

You can't turn anywhere these days in the developer or technology worlds without hearing about AI. Here we avoid the hype cycle with practical learning paths on how to achieve different goals using AI:

- **How to get started with large language models and Node.js**: For JavaScript developers, learn how to access a large language model using Node.js and LangChain.js. You’ll also explore LangChain.js APIs that simplify common requirements like retrieval-augmented generation (RAG).
- **RHEL AI: Try out LLMs the easy way**: Learn how to use Red Hat Enterprise Linux (RHEL) AI experiment with large language models (LLMs) before developing and scaling out to a much larger production environment.
- **From Podman AI Lab to OpenShift AI**: Learn how to rapidly prototype AI applications from your local environment with Podman AI Lab, add knowledge and capabilities to a large language model (LLM) using retrieval augmented generation (RAG), and use the open source technologies on Red Hat OpenShift AI to deploy, serve, and integrate generative AI into your application.
- **Demystify RAG with OpenShift AI and Elasticsearch**: Understand how retrieval-augmented generation (RAG) works and how to implement a RAG workflow using Red Hat OpenShift AI and Elasticsearch vector database.
- **Integrate a private AI coding assistant into your CDE using Ollama, Continue, and OpenShift Dev Spaces**: Learn how to set up a cloud development environment (CDE) using Ollama, Continue, Llama3, and Starcoder2 LLMs with OpenShift Dev Spaces for faster, more efficient coding.

### Virtualization learning paths

Virtualization became a hot topic this year, becoming an area of change and weighing options. Here are learning paths that focus on this theme:

- **OpenShift virtualization and application modernization using the Developer Sandbox**: Learn how to create and manage your virtual machines (VMs) using Red Hat OpenShift and the Developer Sandbox.
- **Windows failover clustering in Red Hat OpenShift Virtualization using SCSI-3 persistent reservation**: Set up clustered storage while running your Windows VMs in a Red Hat OpenShift cluster using the Virtualization operator.
- **Manage OpenShift virtual machines with GitOps**: Learn how to utilize GitOps in OpenShift to manage your VMs.

## Keep learning

Explore learning path hub pages dedicated to the following topics:

- Kubernetes and cloud native
- Linux
- Automation
- Java
- AI/ML

## The rest of the best

Don't miss the rest of our 2024 year in review:

- Ansible automation
- Application development
- AI
- Kubernetes and OpenShift
- Linux
- Programming languages, runtimes, and frameworks

The post Our top learning paths of 2024 appeared first on Red Hat Developer.  
  

Go to Source

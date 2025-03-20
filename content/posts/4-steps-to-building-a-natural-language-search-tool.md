---
title: "4 steps to building a natural language search tool"
date: 2025-02-01
tags: 
  - "development"
  - "git"
  - "github"
  - "gitlab"
  - "software"
---

Empowering humanitarian action with open source: A natural language search tool for UN Resolutions.

The post 4 steps to building a natural language search tool appeared first on The GitHub Blog.

**“We have a problem. Our current search method for sifting through PDFs is extremely manual and time consuming. Is there an easier way?”**

As a developer, this is one of those questions that really gets me excited. I was tasked with finding a way to transform a cumbersome, archival process into an efficient, intuitive search experience. It’s a way to make a group of people’s lives easier, and because of the organizations they work for, help them be more effective in providing humanitarian assistance to people in need around the world. I couldn’t imagine a better project to be working on.

## Unlocking the United Nations’ legacy for rapid action

Since 1945, the United Nations has produced resolutions and other documents that guide international peace and security efforts. Yet accessing this wealth of knowledge remains a challenge, including for organizations such as the International Committee of the Red Cross (ICRC). Currently, delegates at ICRC’s permanent observer mission to the UN advise member states and other stakeholders on international humanitarian law and humanitarian issues. When states negotiate relevant resolutions and other UN products, leaning on pre-existing humanitarian language from UN resolutions can provide precedence. This often requires sifting through PDFs to find relevant content within documents—a time-intensive, manual process ill-suited to the fast-paced world of humanitarian diplomacy.

## A live, accessible, and scalable search platform

To solve this, I built a single-page application (SPA) that enables users to input natural language queries and instantly retrieve relevant UN resolutions. The solution is live now at resolutions.projectrefuge.io and serves as a robust example of how technology can simplify access to critical information.

## How it works

1. **Text extraction and structuring**  
    Using Amazon Textract, I extracted raw text from decades’ worth of UN Security Council Resolutions and Presidential Statements and six years of UN General Assembly Resolutions. A Go script then parsed this text using Regex matching, segmenting it into individual resolutions for easier indexing.
2. **Search-ready database with MongoDB Atlas**  
    I adapted a Node.js script from MongoDB to upload the parsed resolutions as embeddings into a MongoDB Atlas database. This step ensures the content is structured for fast and relevant searches.
    
3. **User interface built with Vue.js**  
    The front end is an intuitive SPA created with Vue.js. Users simply enter semantic search queries—such as “resolutions on humanitarian access in armed conflicts”—and receive results in seconds.
    
4. **Backend hosted on AWS**  
    The backend relies on AWS Lambda and API Gateway, ensuring scalability and seamless performance. The entire application is hosted as a subdomain on AWS Amplify, combining reliability with ease of access.
    

![A Reference Architecture diagram showing the following: Text extraction and structuring Using Amazon Textract, I extracted raw text from decades’ worth of UN Security Council Resolutions and Presidential Statements and six years of UN General Assembly Resolutions. A Go script then parsed this text using Regex matching, segmenting it into individual resolutions for easier indexing. Search-ready database with MongoDB Atlas I adapted a Node.js script from MongoDB to upload the parsed resolutions as embeddings into a MongoDB Atlas database. This step ensures the content is structured for fast and relevant searches. User interface built with Vue.js The front end is an intuitive SPA created with Vue.js. Users simply enter semantic search queries—such as “resolutions on humanitarian access in armed conflicts”—and receive results in seconds. Backend hosted on AWS The backend relies on AWS Lambda and API Gateway, ensuring scalability and seamless performance. The entire application is hosted as a subdomain on AWS Amplify, combining reliability with ease of access.](https://github.blog/wp-content/uploads/2025/01/architecture.png?resize=1024%2C607)

This code is publicly available at projectrefuge/resolutions-search-template. This initiative will empower other organizations to adapt and expand the solution to their unique needs.

https://github.blog/wp-content/uploads/2025/01/20250117-UNresolutiondemo.mp4#t=0.001

## Broader implications: a blueprint for impact

The implications of this project go far beyond the ICRC’s use case with UN Resolutions. With slight modifications, the tool could index and search any collection of legal and policy documents. This approach is a blueprint for organizations aiming to leverage technology for better decision-making and more effective action. For nonprofits, this demonstrates the power of owning your code and building tailored solutions. For developers, it’s a reminder of how open source can accelerate progress in humanitarian and public policy sectors.

## Build together with open source

Projects like resolutions.projectrefuge.io highlight the potential of open source to transform how we access and use information. If you’re a nonprofit, explore GitHub for Nonprofits to discover tools and resources that can help you build your own solutions. Developers eager to contribute to impactful work can browse the For Good First Issue program to find projects that align with their skills and values.

Finally, stay tuned as we work to identify other opportunities with humanitarian actors such as the ICRC to bridge the technology and humanitarian space. Together, we can build a future where knowledge is more accessible and tools are built with collaboration in mind, ensuring that humanitarian efforts are supported by cutting-edge technology.

Let’s code for good—and make a lasting impact.

**If you’d like to lend your developer skills for good,** check out For Good First Issue, a curated platform of open source projects that contribute to a better future for everyone.

The post 4 steps to building a natural language search tool appeared first on The GitHub Blog.

Go to Source

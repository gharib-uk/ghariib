---
title: "Part 1: Creating an Azure storage account and configuring basic settings for security and networking: A step by step guide."
date: 2025-02-01
---

Hey guys, I'm really building some cloud muscles!! From creating a Microsoft Azure account to creating an Azure storage this week has been uttermost eventful. So, this week, here's what I've learned.

First off, What is Azure Storage? Simply put, an Azure storage is a Microsoft cloud service where you store things. Its more like a file cabinet where you can access from anywhere in the world.

## So how do we create an Azure storage?

### Step 1: Create and deploy a resource group to hold all your project resources.

- In the Azure portal, search for and select Resource groups

![Resource group](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fdrtko0a4a2r597qyb2cn.jpg)

- Select + Create

![Create](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fk76x7627psjpdyr41bo9.jpg)

- Give your resource group a name

![Resource group name](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fqicu8a1vk8ns597iy1xg.jpg)

- Select a region. Use this region throughout the project

![Region](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fv1whkoa7vy6p1rybr3o1.jpg)

- Select Review and create to validate the resource group

![Review and create](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F8bdheg8aq1wjzrxhuosh.jpg)

- Select Create to deploy the resource group

![Create](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F57d9nrjo8ltk6x8eo21b.jpg)

### Step 2: Create and deploy a storage account to support testing and training

- In the Azure portal, search for and select Storage accounts.

![Storage account](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Ffv0dncqtdj2g4j5cs391.jpg)

- Select + Create

![Storage account](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fgkfut222ye9p76b0famg.jpg)

- On the Basics tab, select your Resource group and then provide a storage account name. The storage account name must be unique in Azure.

![Storage name](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Ffilqa8u6jpchauo150yw.jpg)

- Set the Performance to Standard then click on Review, and then Create.

![Performance & Standard](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fhxk3oedck2kmnkwnif1k.jpg)

- Wait for the storage account to deploy and then Go to resource.

![Go to resource](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F5zqy6rndfnrlpzmkxx87.jpg)

So we've successfully created a resource group and a storage account. Follow me as we configure simple settings in the storage account in part two of this article.

> Don't forget to give a like, comment and share.

Go to Source

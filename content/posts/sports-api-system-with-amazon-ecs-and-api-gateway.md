---
title: "Sports API System with Amazon ECS and API Gateway"
date: 2025-01-26
---

## Introduction

This project demonstrates the creation of a containerized API management system for querying real-time sports data. The system uses Amazon ECS (Fargate) for container orchestration, Amazon API Gateway for RESTful endpoints, and integrates seamlessly with an external Sports API. The implementation emphasizes best practices in cloud computing, including API management, container orchestration, and secure AWS service integration.

## Prerequisites

**Sports API Key**

Register for a free account at serpapi.com and obtain your API key for accessing sports data.

**AWS Account**

Ensure you have an active AWS account. Familiarity with ECS, API Gateway, Docker, and Python is recommended.

\*_AWS CLI  
\*_  
Install and configure the AWS CLI to enable programmatic interaction with AWS services.

\*_Docker CLI and Desktop  
\*_  
Install Docker CLI and Docker Desktop to build and push container images efficiently.

Project Structure

![Image description](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fh1kthn258fibbuonqwva.png)

Now, let us begin

create a new directory  
`mkdir containerized-sports-api`

Move into the directory  
`cd containerized-sports-api`

Clone my repository  
`git clone https://github.com/Ayindejamiu/sportapiwithecs.git`

create ECR repository  
`aws ecr create-repository --repository-name sports-api --region us-east-1`

Authenticate Build and Push the Docker Image  
\`aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin .dkr.ecr.us-east-1.amazonaws.com

docker build --platform linux/amd64 -t sports-api .  
docker tag sports-api:latest .dkr.ecr.us-east-1.amazonaws.com/sports-api:sports-api-latest  
docker push .dkr.ecr.us-east-1.amazonaws.com/sports-api:sports-api-latest\`

Note: Replace with your AWS Account Id.

After pushing, your screen should be like this

![Image description](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F0e7w2c4wl95p25ufmn7b.png)

**Setup ECS Cluster with Fargate**

Create an ECS Cluster:

Go to the ECS Console → Clusters → Create Cluster

Name your Cluster (sports-api-cluster)

For Infrastructure, select Fargate, then create Cluster

Create a Task definition

Go to Task Definitions → Create New Task Definition

Name your task definition (sports-api-task)

For Infrastructure, select Fargate

Add the container:

Name your container (sports-api-container)

Image URI: go to ECR and check out the private repo. You can find image URI over there.

Container Port: 8080

Protocol: TCP

Port Name: Leave Blank

App Protocol: HTTP

Define Environment Variables:

Key: SERP\_API\_KEY

Value:

\*_Create task definition  
\*_  
Run the Service with an ALB

Go to Clusters → Select Cluster → Service → Create.

Capacity provider: Fargate

Select Deployment configuration family (sports-api-task)

Name your service (sports-api-service)

Desired tasks: 2

Networking: Create new security group

Networking Configuration:

Type: All TCP

Source: Anywhere

Load Balancing: Select Application Load Balancer (ALB).

ALB Configuration:

Create a new ALB:

Name: sports-api-alb

Target Group health check path: "/sports"

Create service

Test the ALB

After deploying the ECS service, note the DNS name of the ALB (e.g., sports-api-alb-.us-east-1.elb.amazonaws.com)

Confirm the API is accessible by visiting the ALB DNS name in your browser and adding /sports at end (e.g, http://sports-api-alb-.us-east-1.elb.amazonaws.com/sports)

![Image description](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fuewfwx3qwsg253jy4xun.png)

**Configure API Gateway**  
Create a New REST API:  
Go to API Gateway Console → Create API → REST API  
Name the API (e.g., Sports API Gateway)  
Set Up Integration:  
Create a resource /sports  
Create a GET method  
Choose HTTP Proxy as the integration type  
Enter the DNS name of the ALB that includes "/sports" (e.g. http://sports-api-alb-.us-east-1.elb.amazonaws.com/sports  
Deploy the API:  
Deploy the API to a stage (e.g., dev)  
Note the endpoint URL

![Image description](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F91dfls4eu469q69tr2z1.png)

Go to Source

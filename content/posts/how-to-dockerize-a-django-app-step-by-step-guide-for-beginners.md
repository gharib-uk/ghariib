---
title: "How to Dockerize a Django App: Step-by-Step Guide for Beginners"
date: 2025-01-08
tags: 
  - "prevents"
---

One of the best ways to make sure your web apps work well in different environments is to containerize them. Containers let you work in a more controlled way, which makes development and deployment easier. This guide will show you how to containerize a Django web app with Docker and explain why it’s a good idea.

We will walk through creating a Docker container for your Django application. Docker gives you a standardized environment, which makes it easier to get up and running and more productive. This tutorial is aimed at those new to Docker who already have some experience with Django. Let’s get started!

![2400x1260 docker evergreen logo blog A](https://www.docker.com/wp-content/uploads/2024/12/2400x1260_docker-evergreen_logo_blog_A-1110x583.png "- 2400x1260 docker evergreen logo blog A")

## Why containerize your Django application?

Django apps can be put into containers to help you work more productively and consistently. Here are the main reasons why you should use Docker for your Django project:

- **Creates a stable environment:** Containers provide a stable environment with all dependencies installed, so you don’t have to worry about “it works on my machine” problems. This ensures that you can reproduce the app and use it on any system or server. Docker makes it simple to set up local environments for development, testing, and production.
- **Ensures reproducibility and portability:** A Dockerized app bundles all the environment variables, dependencies, and configurations, so it always runs the same way. This makes it easier to deploy, especially when you’re moving apps between environments.
- **Facilitates collaboration between developers:** Docker lets your team work in the same environment, so there’s less chance of conflicts from different setups. Shared Docker images make it simple for your team to get started with fewer setup requirements.
- **Speeds up deployment processes:** Docker makes it easier for developers to get started with a new project quickly. It removes the hassle of setting up development environments and ensures everyone is working in the same place, which makes it easier to merge changes from different developers.

## Getting started with Django and Docker

Setting up a Django app in Docker is straightforward. You don’t need to do much more than add in the basic Django project files.

### Tools you’ll need

To follow this guide, make sure you first:

- Install Docker Desktop and Docker Compose on your machine.
- Use a Docker Hub account to store and access Docker images.
- Make sure Django is installed on your system.

If you need help with the installation, you can find detailed instructions on the Docker and Django websites.

## How to Dockerize your Django project

The following six steps include code snippets to guide you through the process.

### Step 1: Set up your Django project

**1\. Initialize a Django project.** 

If you don’t have a Django project set up yet, you can create one with the following commands:

```
django-admin startproject my_docker_django_app
cd my_docker_django_app

```

**2\. Create a `requirements.txt` file.** 

In your project, create a `requirements.txt` file to store dependencies:

```
pip freeze > requirements.txt

```

**3\. Update key environment settings.**

You need to change some sections in the `settings.py` file to enable them to be set using environment variables when the container is started. This allows you to change these settings depending on the environment you are working in.

```
  # The secret key
  SECRET_KEY = os.environ.get("SECRET_KEY")

  DEBUG = bool(os.environ.get("DEBUG", default=0))

  ALLOWED_HOSTS = os.environ.get("DJANGO_ALLOWED_HOSTS","127.0.0.1").split(",")

```

### Step 2: Create a Dockerfile

A Dockerfile is a script that tells Docker how to build your Docker image. Put it in the root directory of your Django project. Here’s a basic Dockerfile setup for Django:

```
# Use the official Python runtime image
FROM python:3.13  

# Create the app directory
RUN mkdir /app

# Set the working directory inside the container
WORKDIR /app

# Set environment variables 
# Prevents Python from writing pyc files to disk
ENV PYTHONDONTWRITEBYTECODE=1
#Prevents Python from buffering stdout and stderr
ENV PYTHONUNBUFFERED=1 

# Upgrade pip
RUN pip install --upgrade pip 

# Copy the Django project  and install dependencies
COPY requirements.txt  /app/

# run this command to install all dependencies 
RUN pip install --no-cache-dir -r requirements.txt

# Copy the Django project to the container
COPY . /app/

# Expose the Django port
EXPOSE 8000

# Run Django’s development server
CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]

```

Each line in the Dockerfile serves a specific purpose:

- `FROM`: Selects the image with the Python version you need.
- `WORKDIR`: Sets the working directory of the application within the container.
- `ENV`: Sets the environment variables needed to build the application
- `RUN` and `COPY` commands: Install dependencies and copy project files.
- `EXPOSE` and `CMD`: Expose the Django server port and define the startup command.

You can build the Django Docker container with the following command:

```
docker build -t django-docker .

```

To see your image, you can run:

```
docker image list

```

The result will look something like this:

```
REPOSITORY      TAG       IMAGE ID       CREATED          SIZE
django-docker   latest    ace73d650ac6   20 seconds ago   1.55GB

```

Although this is a great start in containerizing the application, you’ll need to make a number of improvements to get it ready for production.

- The CMD `manage.py` is only meant for development purposes and should be changed for a WSGI server.
- Reduce the size of the image by using a smaller image.
- Optimize the image by using a multistage build process.

Let’s get started with these improvements.

#### Update requirements.txt

Make sure to add `gunicorn` to your `requirements.txt`. It should look like this:

```
asgiref==3.8.1
Django==5.1.3
sqlparse==0.5.2
gunicorn==23.0.0
psycopg2-binary==2.9.10

```

#### Make improvements to the Dockerfile

The Dockerfile below has changes that solve the three items on the list. The changes to the file are as follows:

- Updated the `FROM python:3.13` image to `FROM python:3.13-slim`. This change reduces the size of the image considerably, as the image now only contains what is needed to run the application.
- Added a multi-stage build process to the Dockerfile. When you build applications, there are usually many files left on the file system that are only needed during build time and are not needed once the application is built and running. By adding a build stage, you use one image to build the application and then move the built files to the second image, leaving only the built code. Read more about multi-stage builds in the documentation.
- Add the Gunicorn WSGI server to the server to enable a production-ready deployment of the application.

```
# Stage 1: Base build stage
FROM python:3.13-slim AS builder

# Create the app directory
RUN mkdir /app

# Set the working directory
WORKDIR /app 

# Set environment variables to optimize Python
ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1 

# Upgrade pip and install dependencies
RUN pip install --upgrade pip 

# Copy the requirements file first (better caching)
COPY requirements.txt /app/

# Install Python dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Stage 2: Production stage
FROM python:3.13-slim

RUN useradd -m -r appuser && 
   mkdir /app && 
   chown -R appuser /app

# Copy the Python dependencies from the builder stage
COPY --from=builder /usr/local/lib/python3.13/site-packages/ /usr/local/lib/python3.13/site-packages/
COPY --from=builder /usr/local/bin/ /usr/local/bin/

# Set the working directory
WORKDIR /app

# Copy application code
COPY --chown=appuser:appuser . .

# Set environment variables to optimize Python
ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1 

# Switch to non-root user
USER appuser

# Expose the application port
EXPOSE 8000 

# Start the application using Gunicorn
CMD ["gunicorn", "--bind", "0.0.0.0:8000", "--workers", "3", "my_docker_django_app.wsgi:application"]

```

Build the Docker container image again.

```
docker build -t django-docker .

```

After making these changes, we can run a `docker image list` again:

```
REPOSITORY      TAG       IMAGE ID       CREATED         SIZE
django-docker   latest    3c62f2376c2c   6 seconds ago   299MB

```

You can see a significant improvement in the size of the container.

The size was reduced from 1.6 GB to 299MB, which leads to faster a deployment process when images are downloaded and cheaper storage costs when storing images.

You could use `docker init` as a command to generate the Dockerfile and `compose.yml` file for your application to get you started.

### Step 3: Configure the Docker Compose file

A `compose.yml` file allows you to manage multi-container applications. Here, we’ll define both a Django container and a PostgreSQL database container.

The compose file makes use of an environment file called `.env`, which will make it easy to keep the settings separate from the application code. The environment variables listed here are standard for most applications:

```
services:
 db:
   image: postgres:17
   environment:
     POSTGRES_DB: ${DATABASE_NAME}
     POSTGRES_USER: ${DATABASE_USERNAME}
     POSTGRES_PASSWORD: ${DATABASE_PASSWORD}
   ports:
     - "5432:5432"
   volumes:
     - postgres_data:/var/lib/postgresql/data
   env_file:
     - .env

 django-web:
   build: .
   container_name: django-docker
   ports:
     - "8000:8000"
   depends_on:
     - db
   environment:
     DJANGO_SECRET_KEY: ${DJANGO_SECRET_KEY}
     DEBUG: ${DEBUG}
     DJANGO_LOGLEVEL: ${DJANGO_LOGLEVEL}
     DJANGO_ALLOWED_HOSTS: ${DJANGO_ALLOWED_HOSTS}
     DATABASE_ENGINE: ${DATABASE_ENGINE}
     DATABASE_NAME: ${DATABASE_NAME}
     DATABASE_USERNAME: ${DATABASE_USERNAME}

     DATABASE_PASSWORD: ${DATABASE_PASSWORD}
     DATABASE_HOST: ${DATABASE_HOST}
     DATABASE_PORT: ${DATABASE_PORT}
   env_file:
     - .env
volumes:
   postgres_data:

```

And the example `.env` file:

```
DJANGO_SECRET_KEY=your_secret_key
DEBUG=True
DJANGO_LOGLEVEL=info
DJANGO_ALLOWED_HOSTS=localhost
DATABASE_ENGINE=postgresql_psycopg2
DATABASE_NAME=dockerdjango
DATABASE_USERNAME=dbuser
DATABASE_PASSWORD=dbpassword
DATABASE_HOST=db
DATABASE_PORT=5432

```

### Step 4: Update Django settings and configuration files

**1\. Configure database settings.** 

Update `settings.py` to use PostgreSQL:

```
  DATABASES = {
       'default': {
           'ENGINE': 'django.db.backends.{}'.format(
               os.getenv('DATABASE_ENGINE', 'sqlite3')
           ),
           'NAME': os.getenv('DATABASE_NAME', 'polls'),
           'USER': os.getenv('DATABASE_USERNAME', 'myprojectuser'),
           'PASSWORD': os.getenv('DATABASE_PASSWORD', 'password'),
           'HOST': os.getenv('DATABASE_HOST', '127.0.0.1'),
           'PORT': os.getenv('DATABASE_PORT', 5432),
       }
   }

```

**2\. Set `ALLOWED_HOSTS` to read from environment files.** 

In `settings.py`, set `ALLOWED_HOSTS` to:

```
   # 'DJANGO_ALLOWED_HOSTS' should be a single string of hosts with a , between each.
   # For example: 'DJANGO_ALLOWED_HOSTS=localhost 127.0.0.1,[::1]'
   ALLOWED_HOSTS = os.environ.get("DJANGO_ALLOWED_HOSTS","127.0.0.1").split(",")

```

**3\. Set the `SECRET_KEY` to read from environment files.**

In `settings.py`, set `SECRET_KEY` to:

```
   # SECURITY WARNING: keep the secret key used in production secret!
   SECRET_KEY = os.environ.get("DJANGO_SECRET_KEY")

```

**4\. Set `DEBUG` to read from environment files.**

In `settings.py`, set `DEBUG` to:

```
 # SECURITY WARNING: don't run with debug turned on in production!
 DEBUG = bool(os.environ.get("DEBUG", default=0))

```

### Step 5: Build and run your new Django project

To build and start your containers, run:

```
docker compose up --build

```

This command will download any necessary Docker images, build the project, and start the containers. Once complete, your Django application should be accessible at `http://localhost:8000`.

### Step 6: Test and access your application

Once the app is running, you can test it by navigating to `http://localhost:8000`. You should see Django’s welcome page, indicating that your app is up and running. To verify the database connection, try running a migration:

```
docker compose run django-web python manage.py migrate

```

## Troubleshooting common issues with Docker and Django

Here are some common issues you might encounter and how to solve them:

- **Database connection errors:** If Django can’t connect to PostgreSQL, verify that your database service name matches in `compose.yml` and `settings.py`.
- **File synchronization issues:** Use the `volumes` directive in `compose.yml` to sync changes from your local files to the container.
- **Container restart loops or crashes:** Use `docker compose logs` to inspect container errors and determine the cause of the crash.

## Optimizing your Django web application

To improve your Django Docker setup, consider these optimization tips:

- **Automate and secure builds:** Use Docker’s multi-stage builds to create leaner images, removing unnecessary files and packages for a more secure and efficient build.
- **Optimize database access:** Configure database pooling and caching to reduce connection time and boost performance.
- **Efficient dependency management:** Regularly update and audit dependencies listed in `requirements.txt` to ensure efficiency and security.

## Take the next step with Docker and Django

Containerizing your Django application with Docker is an effective way to simplify development, ensure consistency across environments, and streamline deployments. By following the steps outlined in this guide, you’ve learned how to set up a Dockerized Django app, optimize your Dockerfile for production, and configure Docker Compose for multi-container setups.

Docker not only helps reduce “it works on my machine” issues but also fosters better collaboration within development teams by standardizing environments. Whether you’re deploying a small project or scaling up for enterprise use, Docker equips you with the tools to build, test, and deploy reliably.

Ready to take the next step? Explore Docker’s powerful tools, like Docker Hub and Docker Scout, to enhance your containerized applications with scalable storage, governance, and continuous security insights.

## Learn more 

- Subscribe to the Docker Newsletter. 
- Learn more about Docker commands, Docker Compose, and security in the Docker Docs. 
- Find Dockerized Django projects for inspiration and guidance in GitHub. 
- Discover Docker plugins that improve performance, logging, and security.
- Get the latest release of Docker Desktop.
- Have questions? The Docker community is here to help.
- New to Docker? Get started.

​Products, containers, Docker, Docker Compose, Docker Hub, dockerfile

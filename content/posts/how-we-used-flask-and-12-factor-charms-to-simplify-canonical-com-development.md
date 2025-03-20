---
title: "How we used Flask and 12-factor charms to simplify Canonical.com development"
date: 2025-01-10
---

Our latest Canonical website rebrand did not just bring the new Vanilla\-based frontend, it also saw the introduction of Python Flask as the preferred web framework. Flask provides a good foundation for building new applications using the 12-factor methodology, effectively increasing our web team’s speed and efficiency in adding new features.

Because our web team works on both the website and the product’s UI we decided to take operational improvements one step further by providing a way to stand up in a few simple commands a fully integrated and observable Kubernetes environment for their Flask apps. Our solution, called 12-factor charms, provides an easy to use abstraction layer over existing Canonical products and it is aimed at application developers who create applications based on the 12-factor methodology.

As part of this blog post we will introduce the 12 factor charms in the context of the Flask, however the same also applies to 12-factor applications that are built using the following frameworks:

- Django
- FastAPI
- Go
- Spring Boot (coming soon)

Let’s explore how you can use 12-factor charms to get a fully integrated and observable Kubernetes environment for Flask apps in a few simple commands.

## The foundations: juju, charms and rocks

The 12-factor charm solution uses and combines capabilities in the following Canonical products:

- Juju is an open source orchestration engine for software operators that enables the deployment, integration and lifecycle management of applications at any scale, on any infrastructure using charms.
- A charm is an operator – business logic encapsulated in reusable software packages that automate every aspect of an application’s life.
- Charmcraft is a CLI tool that makes it easy and quick to initialise, package, and publish Kubernetes and machine charms.
- Rockcraft is a tool to create rocks – a new generation of secure, stable and OCI-compliant container images, based on Ubuntu.

More specifically a Rockcraft framework (conceptually similar to a snap extension) is initially used to facilitate the creation of a well structured, minimal and hardened container image, called a rock. A Charmcraft profile can then be leveraged to add a software operator (charm) around the aforementioned container image.

Encapsulating the original Flask application in a charm allows it to benefit from the entire charm ecosystem, meaning that the app can be connected to a database, e.g. an HA Postgres, observed through a Grafana based observability stack, get ingress and much more.

## Creating a complete development environment in a few commands

Rockcraft and Charmcraft now natively support Flask. Production ready OCI images for Flask applications can be created using Rockcraft with 3 easy commands that need to be run in the root directory of the Flask application:

```
sudo snap install rockcraft --classic
rockcraft init --profile flask-framework
rockcraft pack
```

The full getting started tutorial for creating an OCI image for a Flask application takes you from a plain Ubuntu installation to a production ready OCI image for your Flask application. Using this tooling, the web development team was able to streamline the creation of their OCI images, speeding up their development and reducing maintenance effort.

Charmcraft now also natively supports Flask. The web development team is using it to create charms that automate every aspect of their Flask application’s life, including integrating with a database, preparing the tables in the database, integrating with observability and exposing the application using ingress. From the root directory of the Flask application, the Flask application charm can be created using 4 easy commands:

```
mkdir charm & cd charm
sudo snap install charmcraft --classic
charmcraft init --profile flask-framework
charmcraft pack
```

The full getting started tutorial for creating a charm for a Flask application takes you from a plain Ubuntu installation to deploying the Flask application on Kubernetes, exposing it using ingress and integrating it with a database.

## Learn more about Juju, Rockraft and 12 factor charms

- Juju documentation 
- Rockcraft documentation

Go to Source

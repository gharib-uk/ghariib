---
title: "How To Install and Configure Jenkins on Ubuntu 24.04"
date: 2025-01-02
categories: 
  - "security"
---

Mostly every programmer following a software development life cycle that helps them to keep track of the application. A development life cycle have there own challenges and one of them is to build, test and deploy application. Jenkins is an automation server for the continuous integration tool. It provides a number of plugins for building and deploying your application in an easier way.

You can use Jenkins as a simple CI (Continuous Integration) server or configure this for the CD (Continuous Delivery) hub for any number of the projects. With the help of Jenkins, you can easily distribute work across multiple machines, helping drive builds, tests and deployments across multiple platforms faster.

This tutorial will help you to install and configure Jenkins on Ubuntu 24.04 LTS Linux system. The same instruction can be use for other Ubuntu versions as well. Let’s go through the tutorial to install Jenkins on an Ubuntu system.

## Prerequisites for Jenkins Installation

- Ubuntu server with 22.04 version
- SSH access to server with non-root sudo user
- Java 17 or higher for latest Jenkins (See compatibility chart)
- A web server running Apache or Nginx

## Step 1: Installing Java Development Kit

Jenkins latest version required Java 17 or above versions. The Ubuntu default repositories contains all required JDK packages. Update the Debian packages cache to get latest version definitions.

```
sudo apt update
```

Then install the OpenJDK 17 or higher version on your machine. You can find available versions using “`apt search jdk`” command.

```
sudo apt install openjdk-17-jdk
```

Once JDK installation finished, check the currently active java version.

```
java -version
```

You should she the installed Java version as output.

<figure>

![Java Version for Jenkins](https://tecadmin.net/wp-content/uploads/2016/04/jenkins-java-version.png)

<figcaption>

Java Version for Jenkins

</figcaption>

</figure>

## Step 2: Configure Jenkins PPA

Jenkins officially provides the repositories for package management for the popular operating system. Use the following command to add Jenkins key to your system:

```
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc 
    https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
```

The enable the Jenkins PPA on your Ubuntu system. This repository contains required packages to install Jenkins on Ubuntu Linux.

```
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" 
    https://pkg.jenkins.io/debian-stable binary/ | sudo tee 
    /etc/apt/sources.list.d/jenkins.list > /dev/null
```

## Step 3: Installing Jenkins on Ubuntu

Update the Debian package cache before installing Jenkins on Ubuntu. After that, you can install Jenkins on an Ubuntu system by running below commands.

```
sudo apt update
```

By default Jenkins listens on port 8080, which is very common by multiple applications. If this port is already occupied, you can change this in `/etc/default/jenkins` configuration file by updating value of HTTP\_PORT. Make sure to restart jenkins after changes.

Now verify the Jenkins service status:

```
sudo systemctl status jenkins
```

<figure>

![Installing Jenkins on Ubuntu 24.04](https://tecadmin.net/wp-content/uploads/2016/04/jenkins-service-status-ubuntu2404.png)

<figcaption>

Jenkins Service Status

</figcaption>

</figure>

## Step 4: Initial Jenkins Configuration

Access your server on port **8080** (Or updated port) in your favorite web browser. You will find an wizard to complete initial setup.

1. On the first screen, jenkins will prompt for the initial admin password, You can find that password in `/var/lib/jenkins/secrets/initialAdminPassword` file as shown in below image.
    
    ![Installing Jenkins on Ubuntu 24.04: Step 1](https://tecadmin.net/wp-content/uploads/2024/12/install-jenkins-ubuntu2404-001-1024x593.png)
    
2. Now select appropriate option to install the plugin. You can choose to install suggested plugins or select the required plugins options.
    
    ![Installing Jenkins on Ubuntu 24.04: Step 2](https://tecadmin.net/wp-content/uploads/2024/12/install-jenkins-ubuntu2404-002-1024x606.png)
    
3. I have selected the “install suggested plugins” options. Jenkins will show you the plugins that are installing.
    
    ![Installing Jenkins on Ubuntu 24.04: Step 3](https://tecadmin.net/wp-content/uploads/2024/12/install-jenkins-ubuntu2404-003-1024x599.png)
    
4. Now create an admin account for your Jenkins setup. This will be required to log in to Jenkins.
    
    ![Installing Jenkins on Ubuntu 24.04: Step 4](https://tecadmin.net/wp-content/uploads/2024/12/install-jenkins-ubuntu2404-004-1024x604.png)
    
5. Once you logged in, you will find the dashboard. Where you can create pipeline to build, test and deploy your code.
    
    ![Installing Jenkins on Ubuntu 24.04](https://tecadmin.net/wp-content/uploads/2024/12/install-jenkins-ubuntu2404-005-1024x486.png)
    

## Conclusion

This tutorial helped you to install and initial configuration of Jenkins server on your Ubuntu system. Now you can start build, test and deploy application with CI/CD implementation.

The post How To Install and Configure Jenkins on Ubuntu 24.04 appeared first on TecAdmin.

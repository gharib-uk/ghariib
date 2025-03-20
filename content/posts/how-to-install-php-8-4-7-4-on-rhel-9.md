---
title: "How to Install PHP 8.4-7.4 on RHEL 9"
date: 2025-01-02
categories: 
  - "security"
---

PHP is a popular programming language widely used by developers for creating dynamic and robust web applications. It is known for its simplicity, flexibility, and extensive support for modern frameworks and libraries. Most of the popular CMS applications (eg: WordPress, Drupal etc.) are still using PHP programming language. As of today, the latest PHP version is 8.4, recommended for use in production environments.

REMI is an one of the most trusted RPM repository that provides PHP versions ranging from 7.4 to 8.4. Using this repository, you can easily manage and install multiple PHP versions on your system.

This tutorial will guide you through the installation of PHP 8.4 or any previous version, including PHP 7.4, on a RHEL 9 system.

## Prerequisites

Before starting, ensure you meet the following requirements:

- A running RHEL 9 system
- Shell access with a user that has sudo privileges

## Step 1: Update System Packages

It is always a best practice to ensure your system packages are up-to-date. This helps avoid compatibility issues and ensures system security. The following command will update all existing packages to latest available version in repositories.

```
sudo dnf update
```

Be careful on production servers to fulfill the compatibility of packages with the applications.

## Step 2: Enable RPM Repositories

The RPM packages for php software are still maintained by the REMI repository. Also the REMI repository required the EPEL rpm repository to be configured on system. To install PHP from the REMI repository, you also need to enable the EPEL and REMI both repositories.

- **Use the following command to enable the EPEL repository:**
    
    ```
    sudo dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm
    ```
    
- **To Enable the REMI repository, type:**
    
    ```
    sudo dnf install https://rpms.remirepo.net/enterprise/remi-release-9.rpm
    ```
    

## Step 3: Install PHP 8.4

The REMI repository provides the module for the multiple PHP versions. To install PHP 8.4, enable the corresponding REMI module and proceed with the installation:

```
sudo dnf module enable php:remi-8.4
```

This will enable the PHP 8.4 module and begin the PHP installation. If in case, PHP installation not begins, then type the following command to start PHP installation:

```
sudo dnf install php
```

Once the installation finished, verify the installation by checking the PHP version:

```
php -v
```

The output should display the installed PHP version, confirming a successful installation:

## Step 4: Install Additional PHP Modules

Generally applications relies on multiple additional PHP modules, which is required to be installed on your system. First you need to identify which modules is required for your application. Then search for the available modules using the following command:

```
sudo dnf search php-*
```

For example, to install commonly used modules like GD, MySQL, XML, and SOAP, use:

```
sudo dnf install php-gd php-mysqlnd php-xml php-soap
```

## Step 5: Test PHP Installation

To confirm the PHP installation is working, you can create a test script. Save the following code into a file named `info.php` in your web server’s root directory (e.g., /var/www/html):

```php

<?php phpinfo(); ?> 
```

Access the file in a web browser by navigating to `http://<your-server-ip>/info.php`. If PHP is installed correctly, you will see the PHP information page.

## Conclusion

This tutorial has provided step-by-step instructions to install PHP 8.4 on a RHEL 9 system, along with enabling repositories and installing additional PHP modules. You can now begin developing or deploying web applications using PHP.

The post How to Install PHP 8.4-7.4 on RHEL 9 appeared first on TecAdmin.

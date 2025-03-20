---
title: "How to run SQL Server with SELinux enabled on RHEL 9"
date: 2025-01-20
---

Now you can run SQL Server 2025 as a confined application with pacemaker-enabled SELinux on Red Hat Enterprise Linux (RHEL). This article will demonstrate how to run SQL Server with SELinux enabled on RHEL 9.

SELinux, enabled by default on RHEL 9, ensures enhanced security for your databases. SELinux is a Linux Security Model that defines access controls for applications, processes, and files on a system. It uses security policies, which are a set of rules that tell SELinux what can or cannot be accessed, to enforce the access allowed by a policy. SELinux is particularly valuable in containerized environments, adding an extra layer of isolation between containers and their host systems.

Customers can run their SQL Server 2022 databases according to security best practices with the confidence that Microsoft and Red Hat fully support their configuration. You can try an SELinux-enabled operating system for free. To learn more about running SQL Server 2022 on RHEL 9, check out the article, Install SQL Server on RHEL 9.

As of July 2024, SQL Server 2022 is officially certified with RHEL 9 and generally available on the Red Hat Ecosystem Catalog. 

## How to run SQL Server as a confined application

To run SQL Server as a confined application, make sure SELinux is enabled and in enforcing mode. You can check this by running the following command:

```plaintext
[ ~]$ sestatus SELinux status: enabled SELinuxfs mount: /sys/fs/selinux SELinux root directory: /etc/selinux Loaded policy name: targeted Current mode: enforcing Mode from config file: enforcing Policy MLS status: enabled Policy deny_unknown status: allowed Memory protection checking: actual (secure) Max kernel policy version: 33
```

To identify if the SQL Server service is currently running as confined or unconfined, you can run the command `ps -eZ | grep sqlservr`. If you see the type assigned as `unconfined_service_t`, it means this is running as unconfined.

## Install SQL Server as a confined service

Running SQL Server as an unconfined application is the default mode. But you can still install and run SQL Server as an unconfined application as in previous versions of RHEL. In that case, installation of the `mssql-server-selinux` package is not necessary.

Download the SQL Server 2022 (16.x) Red Hat 9 repository configuration file using this command:

```plaintext
sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/8/mssql-server-2022.repo
```

Install the `mssql-server` package:

```plaintext
sudo yum install -y mssql-server
```

Next install the new `mssql-server-selinux` package:

```plaintext
sudo yum install -y mssql-server-selinux
```

After the package installation finishes, run `mssql-conf setup` using its full path. 

Then follow the prompts to set the system administrator (SA) password and choose your edition as follows:

```plaintext
sudo /opt/mssql/bin/mssql-conf setup
```

Remember to specify a strong password for the SA account. You need a minimum length of 8 characters, including uppercase and lowercase letters, base-10 digits, and/or non-alphanumeric symbols.

We strongly recommend using the SA account to log in for the first time. Users should then immediately disable the SA login as a security best practice. Read more about the SA account here. 

You can verify that this setup is successful by running the following command:

```plaintext
systemctl status mssql-server
```

To allow remote connections, open the SQL Server port on the RHEL firewall. The default SQL Server port is TCP 1433. If you're using FirewallD for your firewall, run the following commands:

```plaintext
sudo firewall-cmd --zone=public --add-port=1433/tcp --permanent
```

```plaintext
sudo firewall-cmd --reload
```

You can verify that SQL Server is running unconfined using the following command:

```plaintext
ps -eZ | grep sqlservr
```

The output should be as follows:

```plaintext
system_u:system_r:unconfined_service_t:s0 48265 ? 00:00:02 sqlservr
```

Once you install the `mssql-server-selinux` package, this will enable a custom SELinux policy that confines the `sqlservr` process. When you install this policy, the `selinuxuser_execmod` Boolean is reset and replaced by a policy named `mssql` which confines the `sqlservr` process in the new `mssql_server_t` domain.

Run this command:

```plaintext
ps -eZ | grep sqlservr
```

This should produce the following output:

```plaintext
system_u:system_r:mssql_server_t:s0 48941 ?      00:00:02 sqlservr
```

For more details refer to the article, Get Started With SQL Server on SELinux. 

## Configure SQL Server as a confined application

You can run the following example playbook against a RHEL 9 system role to configure the SQL Server as a confined application:

```plaintext
- hosts: all
  vars:
    mssql_accept_microsoft_odbc_driver_17_for_sql_server_eula: true
    mssql_accept_microsoft_cli_utilities_for_sql_server_eula: true
    mssql_accept_microsoft_sql_server_standard_eula: true
    mssql_version: 2022
    mssql_password: "p@55w0rD"
    mssql_edition: Evaluation
    mssql_run_selinux_confined: true
    mssql_manage_selinux: true
```

## Next steps

To learn more about installing SQL Server on RHEL 9 and using SELinux, see Install SQL Server on RHEL 9 and Secure SQL Server on RHEL with SELinux. As previously mentioned, SQL Server supports this functionality only on RHEL 9, running the SQL Server version 2022 officially certified on RHEL 9. Feel free to reach out to your Red Hat contacts if you have questions.

The post How to run SQL Server with SELinux enabled on RHEL 9 appeared first on Red Hat Developer.  
  

Go to Source

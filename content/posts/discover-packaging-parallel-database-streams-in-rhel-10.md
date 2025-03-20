---
title: "Discover packaging parallel database streams in RHEL 10"
date: 2025-03-19
---

Red Hat Enterprise Linux, RHEL 8 and RHEL 9 use modularity to distribute various database streams. Modules have many advantages, but also minor drawbacks. RHEL 10 comes with a post-modular solution. This new innovative concept focuses on transparency for users. 

Basic knowledge about using the `dnf` utility is sufficient to start working with a parallel database stream in RHEL 10. This article describes the RHEL 10 post-modular solution for distributing database streams.

## The post-modular solution versus modularity  

Previously in modularity, you needed to handle modular streams by using the `dnf module enable` and `dnf module reset` commands. However, the new post-modular concept does not require such operations.

This new concept incorporates the stream version into the package name as a suffix. As a result, the user only needs the information about the stream version to install. For example, to install PostgreSQL 16, enter:

```plaintext
# dnf install postgresql16
```

The new approach supports the parallel availability of database streams as well as modularity, and it also restricts parallel installation. However, to run multiple streams of database servers on one machine, I prefer containers.

## The default stream feature

For databases that have the default stream defined, namely PostgreSQL and MariDB, you can install the database without the stream version specified in the name. For example, PostgreSQL 16 is the default stream for RHEL 10, and you can install it by using one of the following commands:

```plaintext
# dnf install postgresql16
```

```plaintext
# dnf install postgresql
```

This feature is not present in MySQL due to the shorter support of each stream. The following table lists the initial RHEL 10 stream for each database system.

|   Database   |   Initial (default) stream    |   RPM name   |   Default stream RPM name   |
| --- | --- | --- | --- |
|   PostgreSQL   |   16   |   postgresql16   |   postgresql   |
|   MariaDB   |   10.11   |   mariadb10.11   |   mariadb   |
|   MySQL   |   8.4   |   mysql8.4   |   no-default   |

The modules in RHEL 8 and RHEL 9 (i.e., PostgreSQL modules) contain PostgreSQL extensions. 

Supported extensions:

- `postgres-decoderbufs`
- `pg_repack`
- `pgaudit`
- `pgvector`

These extensions are also present in RHEL 10. You  can install them by using `<db_name_stream>-<extension_name>` as the package name_._ For the default stream, the name can be specified without the version. For example, both of the following commands install the `pgvector` extension for the default stream 16 for PostgreSQL: 

```plaintext
# dnf install postgresql16-pgvector 
```

```plaintext
# dnf install pgvector
```

## The RHEL 10 post-modular solution

Welcome to the new era of packaging parallel database streams in RHEL 10. In this article, I described the Red Hat Enterprise Linux 10 post-modular solution for distributing database streams and the advantages of the default streams feature. 

To learn more about RHEL 10, refer to the release notes. Explore the RHEL offerings by downloading RHEL and RHEL 10 beta.

The post Discover packaging parallel database streams in RHEL 10 appeared first on Red Hat Developer.  
  

Go to Source

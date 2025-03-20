---
title: "Get Valkey security patching and support with Ubuntu Pro"
date: 2025-01-02
categories: 
  - "general"
---

Canonical is pleased to announce security patching and support for Valkey through the Ubuntu Pro subscription. Ubuntu Pro is a subscription for open source software security, compliance and support that expands the maintenance period for all packages distributed in Ubuntu repositories. This extension applies to all the open source software on Ubuntu, including Valkey.

This means that Valkey users can now benefit from the following features offered with Ubuntu Pro:

- Valkey 7.2.5 is the drop-in replacement for the last open source version of Redis before the adoption of the dual source-available license.  This version will be supported by Canonical for up to 12 years. This means that Canonical will be dedicated to the stability of Valkey through continuous CVE patching and backporting.  
    
- Attaching the Ubuntu Pro subscription to Ubuntu brings you additional enterprise-grade features, including Linux kernel livepatching, access to FIPS-validated packages, and compliance with security profiles such as CIS. Using Valkey on Ubuntu ensures that the OS and the kernel, where deployed, have automated patching and maintenance, and live kernel updates to minimize application downtime.  
    
- Ubuntu Pro is available as a single subscription, which makes it well-suited to enterprises looking to scale and run multiple production servers and desktop environments. Individual users can also take advantage of Ubuntu Pro benefits, as it is free for up to 5 machines.

## What is Valkey?

Valkey is an open source key/value data store licensed under the BSD 3-clause license. Organizations choose Valkey because it supports a variety of workloads, including caching, in-memory databases, message queueing, and leaderboards. It can function either as a standalone server or as part of a cluster, providing replication options that ensure high availability and data redundancy.

Valkey is seeing increased adoption in enterprise use cases. It is an excellent choice for e-commerce websites that need a database to manage high traffic, expanding product catalogs, and large transaction volumes. As an in-memory database, Valkey delivers ultra-fast performance, a scalable architecture, and real-time updates, making it ideal for modern web applications. Other use cases for Valkey include session management, where it is used to provide fast and reliable access to user sessions, and real-time analytics, where it enables processing and response to live data streams with minimal latency.

## Enable   Ubuntu Pro to get security patching for Valkey

There are two simple steps to benefit from security coverage for Valkey: first, subscribe to Ubuntu Pro with the guide below. Afterwards, run Valkey with the Ubuntu Pro subscription enabled. This security coverage is currently available for Ubuntu 24.04 LTS.

To get started with Ubuntu Pro, follow these standard Ubuntu Pro steps along with the guide for Valkey installations below:

1\. Select a subscription mode. You can choose either organisation or personal.

2\. Specify the types of machines you plan to set up. You can run Valkey on any.

3\. Indicate the number of machines you want to run with Ubuntu Pro.

4\. Choose the Ubuntu Pro LTS version; select 24.04.

5\. Choose the repository coverage—select “Pro all repositories” to access the Valkey repository.

6\. Identify your needs for phone and ticket support. You can choose any option.

7\. After completing the above steps, you will be redirected to a checkout button to either start a trial or purchase a subscription.  

### Run Valkey with an  Ubuntu Pro subscription

Once Valkey is installed on your system (\`apt-get install valkey\`), its daemon will be running automatically, and your application will be able to connect and interact with it. Users can do that with Valkey’s CLI or one of the bindings for multiple programming languages we have available.

Use Valkey in the Command Line Interface (CLI):

```
$ valkey-cli -h 127.0.0.1 -p 6379 GET <variable_name> <value>

$ valkey-cli -h 127.0.0.1 -p 6379 GET <variable_name>

```

The options passed to the CLI above are:

- \`-h\`: receives the IP address of the host running the server, in this case it is localhost;
- \`-p\`: receives the port that the service is binded to, and by default, Valkey runs on port 6379;
- \`SET\`: sets a key/value, and receives the key name and its value; 
- \`GET\`: command to read the value of a given key.

The current version of Valkey in Ubuntu is fully compatible with the last open source version of Redis. Any  Redis library can be used to connect and interact with a Valkey server using a Python library (\`apt-get install python3-redis\`), for instance. 

```
Python

import redis as valkey

client = valkey.StrictRedis(host='127.0.0.1', port=6379)

client.set('key1', 'value1')

print('key1.value :', client.get('key1'))
```

## Conclusion 

By including Valkey in Ubuntu Pro, Canonical is reaffirming its commitment to supporting open source projects in the wider community. We’re looking forward to helping Valkey users take their projects forward with expanded security coverage.  

To get started with Ubuntu Pro, visit our instructional page, and be sure to follow the installation guidance set out in this blog. 

Learn more or contact our team.

Go to Source

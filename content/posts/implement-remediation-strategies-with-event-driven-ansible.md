---
title: "Implement remediation strategies with Event-Driven Ansible"
date: 2025-01-10
---

Event-Driven Ansible is a powerful extension to Red Hat Ansible Automation Platform that leverages the automation infrastructure to provide the ability to react to change or problems. In short, Event-Driven Ansible can trigger Ansible playbooks (or Ansible Automation Platform's Job Templates) if a certain event is detected.

In this article, we will provide a series of examples on how to use Event-Driven Ansible to implement several remediation strategies based on activities in the environment. We'll use Red Hat JBoss Enterprise Application Platform (JBoss EAP), part of Red Hat Runtimes, as a reference implementation.

## Environment set up: Podman

To keep it simple and easy to reproduce, we will set up our Ansible controller inside into a Podman container based on the latest Fedora release. As this instance will need to communicate with other instances, we'll start by creating a network for those containers to coexist:

```plaintext
$ podman network create eda_red_hat_blog
```

With this network created, we can set up our first container—the instance that we will use as an Ansible **control node**:

```plaintext
$ podman run -it --name=control_node --rm --network eda_red_hat_blog registry.fedoraproject.org/fedora:latest /bin/bash
```

### Prepare the control node

Once the container is running, Ansible can be installed:

```plaintext
# dnf install ansible-core
```

The Ansible collection for EAP is provided by Ansible automation hub. In order to use it as a content provider, you'll need to update the `ansible.cfg` of your project. For more information, refer to the Ansible automation hub documentation.

Then, install the necessary Ansible collections: Ansible collection for Event-Driven Ansible and Ansible collection for EAP:

```plaintext
$ ansible-galaxy collection install ansible.eda redhat.eap
```

**Note**: Of course, this control node set up is not required if an Ansible Automation Platform is available and is being used. The approach described within this article (using a Podman container and a Fedora distribution) is an easy way to reproduce the demo without such infrastructure at our fingertips.

### Target

With the control node set up, a target node needs to be available. We'll use a UBI9 container to emulate a system running with Red Hat Enterprise Linux (RHEL) 9:

```plaintext
$ podman run --rm -d --network eda_red_hat_blog --systemd=true --privileged=true --name=node registry.access.redhat.com/ubi9/ubi-init:latest
```

Note that we use the `ubi-init` image, as this flavor of UBI9 is already preconfigured to run `systemd` and its services within the container.

We'll need to install and activate SSHd so that our control node can connect to the container representing the node:

```plaintext
$ podman exec -ti node /bin/bash
# dnf install -y openssh-server
# # for the demo, we'll authorize root login. Obviously don't do this for sensitive or production systems!!!
# sed -i /etc/sshd/sshd_config -e '/^.*PermitRootLogin.*$/PermitRootLogin yes/'
# systemctl start sshd
```

Before going any further, let's check that the container representing the control node can indeed connect to the target. First, we need to create an inventory file for Ansible so that it can identify the target:

```plaintext
$  cat inventory
[all]
node
```

Now, let's run Ansible and verify that it can connect to the remote host. In this case, we'll simply ask the automation tool to gather the available information (facts in Ansible parlance) about the remote system:

```plaintext
$ ansible -m setup -i inventory all
```

This command will return a lot of information about the target, structured as YAML content. At this stage, it’s enough to conclude that Ansible is properly configured and able to connect to the remote node.

## Set up JBoss EAP

We are now going to set up JBoss EAP as a service on the target using the automation provided by the Ansible Collection for EAP we installed earlier on the control node. This will provide a suitable service to monitor using Event-Driven Ansible.

### Credentials to download EAP

As JBoss EAP is a Red Hat product, we need to provide `service account` credentials to Ansible. This will allow the automation tool connect to the Red Hat Customer Portal and download the appropriate archive. To accomplish this task, the required information can be placed into a YAML file and referenced by Ansible at runtime:

```plaintext
    ---
    rhn_username: <service_account_username>
    rhn_password: <service_account_password>
```

For more information on how to obtain those credentials, please refer to the following documentation.

### Install JBoss EAP

Thanks to the Ansible Collection for EAP, the playbook to deploy this product is minimal:

```python
---
- name: Ensure Wildfly is install and running as a service
  hosts: all
  collections:
    - redhat.eap
  roles:
    - eap_install
    - eap_systemd
```

We just need to provide Ansible the aforementioned credentials and, from there, the automation tool will take care of the rest:

```plaintext
$ ansible-playbook -i inventory playbook.yml -e @service_acount.yml
```

**Note**: The collection will not download the JBoss EAP archive each time its content is executed. It will only download from Red Hat Customer Portal once and, from there, as long as the archive exists on the file system of the control node, the archive will no longer be downloaded.

Once Ansible has finished running, we can check that the service was indeed installed and properly running by using `systemctl status eap` command. The playbook below can be used to perform this verification from the control node:

```python
---
- name: Ensure EAP is installed and running as a service
 hosts: all
 gather_facts: false
 tasks:
    - ansible.builtin.command: "systemctl status eap"
     register: status_eap
    - ansible.builtin.debug:
       var: status_eap
     when:
       - status_eap is defined and status_eap.stdout is defined
```

## JBoss EAP's remediation using Event-Driven Ansible

Now that we have our target node provisioned, we can start looking at Event-Driven Ansible a bit more closely.

### Install ansible-rulebook on the controller

First, we'll need to install the `ansible-rulebook` command on the control node. Within the Fedora container, like the one we set up, we'll need to use `pip` to do so:

```plaintext
# dnf install python3-pip.noarch
# pip install ansible-rulebook
# ansible-rulebook --version
 # ansible-rulebook --version
 1.1.1
    Executable location = /usr/local/bin/ansible-rulebook
    Drools_jpy version = 0.3.9
    Java home = /usr/lib/jvm/java-17-openjdk-17.0.13.0.11-3.fc41.x86_64
    Java version = 17.0.13
     Python version = 3.13.0 (main, Oct  8 2024, 00:00:00) [GCC 14.2.1 20240912 (Red Hat 14.2.1-3)]
```

Note that this step is not required if the user has, at their disposal, an Ansible Automation Platform instance (with Event-Driven Ansible installed).

### Use case 1: Restart EAP if service is down

Now that Event-Driven Ansible is fully available on our control node, we can use it to detect if the JBoss EAP service on the target node is not running. When such an event occurs, Event-Driven Ansible will trigger automation in order to restart the service. It's a simple use case, but it's an excellent starting point to discover the capabilities of Event-Driven Ansible. If traditional Ansible automation uses playbooks to define its actions, Event-Driven Ansible uses rulebooks—both using the YAML syntax. Because of the similarities between the two, the content looks familiar to most Ansible users.

A rulebook is split into two sections. The first one is labeled `sources` and provides, as its name implies, the source of events to potentially take action on. This is where we can integrate Event-Driven Ansible with numerous systems that supplied events (such as Apache Kafka, for instance). The second part, labeled `rules`, describes the actions to be executed if a particular event has been identified.

In our case, we will use a built-in `source`, named `check_url`. As its name implies, it will simply monitor a URL, the web root context of JBoss EAP. If this is not reachable, then JBoss EAP is most likely down. As for the rule, we will execute the following playbook to ensure the service is (re)started:

```plaintext
---
- name: Ensure EAP is installed and running as a service
 hosts: node
 gather_facts: false
 tasks:
    - name: Restart EAP
       ansible.builtin.service:
           name: eap
               state: started
```

Now, let's review the content of our first rulebook:

```plaintext
---
- name: EAP remediation using EDA
  hosts: all
  sources:
    - ansible.eda.url_check:
          urls:
             - "http://node:8080/"
   rules:
      - name: Restart EAP if url is not reachable
        condition: event.url_check.status == "down"
        action:
          run_playbook:
            name: playbooks/restart_eap.yml
```

Since the format is similar to a playbook, the content is pretty straightforward and easy to understand. The top portion of the rulebook mirrors that of a playbook, as it includes the name of the rulebook and the targets (hosts).

As described previously, an entry for sources and one for rules is also included. The source included within this rulebook, provided by Event-Driven Ansible, checks that a URL is reachable and generates events based on the state of the remote system.

The `rules` property includes the list of rules that may trigger actions based on events generated within the list of sources. The rule included in this rulebook will trigger a playbook to restart JBoss EAP if the exposed URL is no longer reachable.

Pretty simple and neat!

Now, let’s run this rulebook using the following command:

```plaintext
$ ansible-rulebook -r rulebooks/rulebook1.yml -i inventory
```

Once started, the command will wait for an event to match one of the rules based on the conditions defined. The condition, where the URL fails to return a successful result, can be triggered by simply using the `systemctl stop eap` command on the target node.

Then, we can see that Event-Driven Ansible picks up the event and attempts to remediate the situation by restarting the JBoss EAP service:

```plaintext
$ ansible-rulebook -r rulebooks/rulebook1.yml -i inventory
PLAY [Ensure EAP is installed and running as a service] **************************
TASK [Restart EAP] *************************************************************
changed: [node]
```

### Use case 2: Restart EAP in case of Out of Memory error

Restarting a service is a rather simple use case. Now, we are going to review a more complex, real life use case. Let's imagine that one of the applications hosted on JBoss EAP sometimes throws an `Out of Memory error`. The developers have linked the problem to the database connection pool and a way to remediate the situation is to flush the idled connections within JBoss EAP.

We are now going to use Event-Driven Ansible to automate this remediation strategy. We'll use one of the other event sources that is provided by Event-Driven Ansible: `journald`. This allows one to react to events logged in to `journald` on the target instance.

As the Ansible Collection for EAP integrates the Java server into `systemd` during its installation, its own log entries are propagated into the system it is running within. So, we can use this source to detect the appearance of the infamous error message `java.lang.OutOfMemoryError`.

Once this event is identified, Event-Driven Ansible will trigger another playbook called `flush_idle_conn.yml`. The playbook will leverage the capabilities of the Ansible Collection for EAP to interact with the Java server using its CLI feature.

The following is the content of the `flush_idle_conn.yml`:

```plaintext
---
- name: "Flus idle connection"
  hosts: all
  collections:
    - redhat.eap
  tasks:
    - name: Flush idle connection
      ansible.builtin.include_role:
        name: wildfly_utils
        tasks_from: jboss_cli.yml
      vars:
        jboss_cli_query: "/subsystem=datasources/data-source=ExampleDS:flush-idle-connection-in-pool()"
```

And here is our new rulebook:

```plaintext
---
- name: Remediate Out of Memory error triggered by idle connection
   hosts: all
   sources:
     - ansible.eda.journald:
        match: ALL
   rules:
     - name: Match all on journald
       condition: event.journald.message is search("java.lang.OutOfMemoryError", ignorecase=false)
       action:
         run_playbook:
           name: playbooks/flush_idle_conn.yml.yml
```

Since constructing this scenario goes beyond the scope of this discussion, the execution of this rulebook and the associated remediation will not be included. However, the concepts provide a glimpse into the types of strategies that can be incorporated within Event-Driven Ansible.

# Summary

As demonstrated in this article, Event-Driven Ansible is a powerful way to implement remediation strategies (or other events based operations) upon activities within the environment. In the case of the Red Hat Runtimes solutions, such as JBoss EAP, JWS, or even Red Hat Build of Keycloak, the available collections on Automation hub ensure an easy integration with both Ansible and Event-Driven Ansible.

The post Implement remediation strategies with Event-Driven Ansible appeared first on Red Hat Developer.  
  

Go to Source

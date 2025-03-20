---
title: "Strategies for eliminating Ansible hardcoded credentials"
date: 2025-01-23
---

Ansible execution environments provide a reusable and supported operating environment for running Red Hat Ansible Automation Platform. These container\-based instances provide everything that the desired automation needs to execute successfully, including any of the necessary dependencies and supporting components, while also eliminating many of the challenges faced when running automation at scale. During the process of building execution environments, there may be a need to access content from sources that require authentication. Hardcoding these properties into the build process may present a security concern and does not align with recommended operating practices. In this article, I will present several areas where hardcoded properties are typically used when building execution environments and the alternative options available to produce software more securely.

## Execution environments and security

Execution environments comprise several different components including operating system packages, Python libraries, and Ansible collections. Their composition is expressed within a definition file (by default with the name `execution-environment.yml`) and Ansible Content Collections represent the primary content source within an execution environment. Collections can originate from a variety of sources including Ansible Galaxy, Red Hat Automation Hub, or a private instance Automation Hub. For the latter two, authentication is typically required in order to obtain the automation content. Accessing automation content stored in remote sources is typically defined within an `ansible.cfg` file containing Ansible configuration settings and includes remote Galaxy server and any other authentication details, when required.

Collections within an execution environment definition file can either be specified within an external file within the `dependencies.galaxy` property or defined explicitly within the `dependencies.galaxy.collections` property, as shown in the example below:

```plaintext
---
version: 3

...

dependencies:
  # Explicitly referencing collections to install
  galaxy:
    collections:
      - <collection1>
      - <collection2>

  # Reference collections within an external file
  galaxy: <file>
```

Remote Galaxy servers that are included within an `ansible.cfg` file are defined within sections prefixed with `galaxy` as shown below:

```plaintext
[galaxy]
server_list = upstream_galaxy, automation_hub_certified

[galaxy_server.upstream_galaxy]
url=https://galaxy.ansible.com/

[galaxy_server.automation_hub_certified]
url=https://console.redhat.com/api/automation-hub/content/published/
auth_url=https://sso.redhat.com/auth/realms/redhat-external/protocol/openid-connect/token
token=<token>
```

The list of Ansible Galaxy servers is defined within the `server_list` property within the galaxy section. In the example above, `upstream_galaxy` and `automation_hub_certified` refer to the publicly available Ansible Galaxy instance and the set of certified content within Red Hat’s public instance of Automation Hub. The configuration for each server is specified in a section using the format `galaxy_server.<server_name>`. 

For repositories that are accessible without the need to define any credentials, only the `url` property of the remote instance is required. However, if authentication is required, either the `username` and `password` properties must be defined when retrieving automation content using basic authentication, or the `auth_url` and `token` properties must be provided when leveraging OpenID Connect (OIDC)-based authentication. (Other options are available, but may not be required in all cases.) Regardless of the desired authorization type, specifying sensitive assets within this file, such as a password or token, represents a security risk. Instead, these configurations can be externalized so that they are neither defined nor hardcoded within the `ansible.cfg` file.

Each of the properties in the example above within an `ansible.cfg` file can be externalized as environment variables. The names of the variables from the sections starting with `galaxy_server` take the form `ANSIBLE_<section_id>.<property>` (all uppercased). So, for example, the URL for the upstream Ansible Galaxy server is represented with the variable name `ANSIBLE_GALAXY_SERVER_UPSTREAM_GALAXY_URL`. The list of servers within the galaxy section is defined in the environment variable named `ANSIBLE_GALAXY_SERVER_LIST`.

## Manageability

Aside from the security benefits, externalizing the properties associated with remote repositories also helps from a manageability perspective. As an execution environment is built, several phases take place in order to produce the final image. In order to make the `ansible.cfg` file visible/available to those steps, additional actions need to be specified within the execution environment definition file in order to place the file in the appropriate location so that it can be used during the build. Of course, this is on top of just managing the `ansible.cfg` file itself. Instead, the content of the environment variables can either be defined within the execution environment definition file or injected at build time.

To gain greater control of the execution environment build process, the `additional_build_files` property of the execution environment definition file can be utilized to accomplish the goal of using environment variables to represent Galaxy Server configurations. Several build phases are available for customization, but the `prepend_galaxy` step allows actions to be configured prior to building the Galaxy image. These actions are standard Dockerfile/Containerfile commands, such as `RUN` and `COPY`, and environment variables, like those that were discussed previously, can be defined using the `ENV` instruction.

The non-sensitive values can be defined within the execution environment definition file to replace their analogous values from within the `ansible.cfg` file, as shown below:

```plaintext
  prepend_galaxy:
    - ENV ANSIBLE_GALAXY_SERVER_LIST=automation_hub_certified,upstream_galaxy
    - ENV ANSIBLE_GALAXY_SERVER_UPSTREAM_GALAXY_URL=https://galaxy.ansible.com/
    - ENV ANSIBLE_GALAXY_SERVER_AUTOMATION_HUB_CERTIFIED_URL=https://cloud.redhat.com/api/automation-hub/
    - ENV ANSIBLE_GALAXY_SERVER_AUTOMATION_HUB_CERTIFIED_AUTH_URL=https://sso.redhat.com/auth/realms/redhat-external/protocol/openid-connect/token
```

Sensitive values, like the token that is used to authenticate to Automaton Hub, can be injected at runtime to avoid any trace from being statically defined. `ansible-builder` is the tool that is used to build and produce execution environments, and it includes the capability to specify arguments that should be included at build time using the `--build-arg flag`. To utilize a build argument as an environment variable within the execution environment definition file, the `ARG` instruction can be used.

**Note**: The `ansible-navigator` tool can also be used to build and produce execution environments as it includes the capabilities of `ansible-builder` using the `builder` subcommand. See below:

```plaintext
    - ARG ANSIBLE_GALAXY_SERVER_AUTOMATION_HUB_CERTIFIED_TOKEN
```

Notice how not only is the `ARG` instruction used, but no value is defined, as it will be injected at build time.

To demonstrate and verify that the Ansible Galaxy server related details have been transitioned successfully from the `ansible.cfg` file to environment variables within the execution environment definition file enabling the externalization of sensitive properties, create an `execution-environment.yml` file with the following content:

```plaintext
---
version: 3

images:
  base_image:
    name: registry.redhat.io/ansible-automation-platform-25/ee-minimal-rhel9:latest

options:
  package_manager_path: /usr/bin/microdnf  # downstream images use non-standard package manager

dependencies:
  # Collections to be installed using Galaxy
  galaxy:
    collections:
      - ansible.controller
      - community.general 

additional_build_steps:
  prepend_galaxy:
    - ENV ANSIBLE_GALAXY_SERVER_LIST=automation_hub_certified,upstream_galaxy
    - ENV ANSIBLE_GALAXY_SERVER_UPSTREAM_GALAXY_URL=https://galaxy.ansible.com/
    - ENV ANSIBLE_GALAXY_SERVER_AUTOMATION_HUB_CERTIFIED_URL=https://console.redhat.com/api/automation-hub/
    - ENV ANSIBLE_GALAXY_SERVER_AUTOMATION_HUB_CERTIFIED_AUTH_URL=https://sso.redhat.com/auth/realms/redhat-external/protocol/openid-connect/token
    - ARG ANSIBLE_GALAXY_SERVER_AUTOMATION_HUB_CERTIFIED_TOKEN
```

The `community.general` collection from the upstream Ansible Galaxy server and the `ansible.controller` collection from Red Hat Automation Hub are the two dependencies that will be included in this execution environment.

Now, build the execution environment image with `anisble-builder` using the following command:

```plaintext
ansible-builder build -t localhost/ee-galaxy-env-vars:latest -v=3 --build-arg ANSIBLE_GALAXY_SERVER_AUTOMATION_HUB_CERTIFIED_TOKEN=<TOKEN>
```

Replace `<TOKEN>` with the value obtained from the Red Hat Hybrid Cloud Console. This can be stored in a secure facility, such as Ansible Vault or an external secrets management tool.

Once the build has been completed, use the `ansible-navigator` tool to confirm the collections were included successfully:

```plaintext
ansible-navigator collections --pp=missing --eei=localhost/ee-galaxy-env-vars
```

Note that both the `ansible.controller` and `community.general` collections are present, indicating that they not only were installed successfully, but that the token associated with Automation Hub was injected into the build successfully. See below:

```plaintext
  Name                     Version    Shadowed    Type         Path
0│ansible.builtin          2.15.12    False       contained    /usr/lib/python3.9/site-packages/ansible
1│ansible.controller       4.6.2      False       contained    /usr/share/ansible/collections/ansible_collections/ansible/controller
2│community.general        10.0.1     False       contained    /usr/share/ansible/collections/ansible_collections/community/general
```

Hit the **ESC** key to exit out of the interactive terminal session spawned by Ansible Navigator.

## Conclusion

Execution environments are the preferred method of running Ansible automation content and is how automation activities are carried out within Ansible Automation Platform. By eliminating the need to explicitly define sensitive assets within execution environment definition files and to externalize these values as environment variables which are injected at runtime, the build process is simplified and security is increased.

Interested in learning more about building and utilizing Ansible Execution Environments in your own automation workflows? Explore both the associated documentation as well as interactive exercises to level up your skills:

Documentation:

- Creating and Using Ansible Execution Environments
- Using Ansible Content Navigator

Interactive exercises:

- Getting started with Ansible Builder
- Getting started with Ansible Navigator

The post Strategies for eliminating Ansible hardcoded credentials appeared first on Red Hat Developer.  
  

Go to Source

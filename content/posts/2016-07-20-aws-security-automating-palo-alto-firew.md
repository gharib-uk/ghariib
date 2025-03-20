---
title: "AWS Security Automating Palo Alto firewall rules with AWS Lambda"
date: 2016-07-19T23:40:00.000-04:00
draft: false
type: posts
---
# AWS Security Automating Palo Alto firewall rules with AWS Lambda

<br/>

<br/>
**

With the increased adoption of IaaS cloud services such as Amazon Web Services (AWS) and Microsoft Azure, there is also a greater need for security controls in the cloud. Firewall and IPS vendors such as Palo Alto, Checkpoint and Fortinet have made available virtual instances of their products ready to run in these cloud environments. These tools can provide great advantages on top of the existing security controls inherent in the cloud platform, and can provide more inspection capabilities and filtering, especially at the application level. In addition to firewall capabilities, these products can provide extra features such as intrusion prevention, URL filtering, and other features that are lacking with the native security controls.

However, there is still lots of manual configuration expected from the security or network administrators, such as configuring interfaces or network settings, firewall and threat prevention rules, and so on. But in a cloud environment where speed and flexibility are expected, waiting for a CAB meeting, change window, etc. for a newly created instance can seem to be a step back. Firewall vendors are definitely aware of this, and have introduced features such as Palo Alto’s ability to read AWS attributes (for example tags or instance IDs) and use them in dynamic rules that get updated as changes occur in the cloud environment. For example, you can create a group with the setting to include anything with a certain tag, and then use this group in a security rule to allow Web traffic to it. The gateway will then know to allow web traffic to any new or existing instance that has this tag. A full list of monitored AWS attributes can be found [here](https://www.paloaltonetworks.com/documentation/71/pan-os/pan-os/policy/attributes-monitored-in-the-aws-and-vmware-environments#59798).

But there are still more features that you might want to configure automatically that are not included by the vendor. The vendors mentioned above all have API interfaces available, and so combining that with the tools from Microsoft or Amazon, we can easily write small pieces of code to automate lots of these tasks. To demonstrate this, I wrote a lambda function that monitors AWS instance starts and stops, as well as security group updates, and then pushes or deletes rules to a Palo Alto gateway based on these changes.

In this post, I will go over the different components of my [aws-lambda-paloalto](https://github.com/akittana/aws-lambda-paloalto) code, its design, and then show it in action. Finally i will list a number of notes and considerations as well as a link to download the code.



**
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Components
----------

### Palo Alto

**

Palo Alto instances can be accessed from the [Amazon marketplace](https://aws.amazon.com/marketplace/seller-profile?id=0ed48363-5064-4d47-b41b-a53f7c937314). Palo Alto provides excellent documentation on how to set up a gateway in the AWS, and I would recommend to start [here](https://www.paloaltonetworks.com/documentation/71/virtualization/virtualization/set-up-the-vm-series-firewall-in-aws/deploy-the-vm-series-firewall-in-aws#62809) for the initial configuration. Another useful [case study](https://www.paloaltonetworks.com/documentation/71/virtualization/virtualization/set-up-the-vm-series-firewall-in-aws/use-case-use-dynamic-address-groups-to-secure-new-ec2-instances-within-the-vpc#73257) provided by Palo Alto is on how to configure and use dynamic address groups in rules, where the groups are based on AWS attributes.

For this setup, I had a Palo Alto gateway configured as an Internet gateway in AWS, and so all internet traffic from my instances was passing through it. I also had an elastic IP assigned to the management port, and my lambda function used this IP address to configure the gateway. Using a public IP address to configure the gateway is not the best option, and I would recommend using a private IP instead (I address some of the limitations this might introduce in the notes section below).



**
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### AWS CloudTrail/S3

**

CloudTrail is a service from AWS to log and store the history of all API calls made in an AWS environment. CloudTrail saves all the logs to AWS S3 which is another AWS service that provides object storage. CloudTrail has to be enabled so that we can monitor when changes are made that are relevant to our code, and then act based on that information.

More details on CloudTrail and S3 can be found [here](https://aws.amazon.com/cloudtrail/) and [here](https://aws.amazon.com/s3/).



**
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### AWS Lambda

**

Lambda is a service from AWS that lets you upload code, and AWS will run it for you based on triggers you set up,such as triggers from other AWS services or external triggers, without having to run a dedicated instance for your code. In our case, we will use CloudTrail (S3) as a trigger for our code so that whenever a new change is made in the environment, the code can scan the changes and determines if a corresponding change is required in the Palo Alto gateway.

Details on AWS Lambda can be found [here](https://aws.amazon.com/lambda/).



**
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Code Features/Design
--------------------

### Adding rules

**

The lambda function that will monitor the following two events for adding new rules:

-   StartInstances: Event indicating that a new instance was started
    
-   AuthorizeSecurityGroupIngress: Event indicating that a new rule was adding to an existing security group
    

Once any of these two events are detected, the Lambda function will extract the relevant information for rules required to the instances affected, and add the corresponding rules to our Palo Alto instance.

The script has been designed to add rules with the name corresponding to the type of event that triggered them. For example, if the rule added is because of an instance with instance-id X, then the rule name is  ‘X-#’ where # is increased with every rule added. Correspondingly,If the Lambda function executes due to a change in a security group with group-id Y, then the naming is ‘Y-#’. This naming convention is used by the code to track the rules it added.



**
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### Unnecessary rules

**

Since the Palo Alto gateway is running as an internet gateway, there are many scenarios that are not relevant, and the code will try to filter out these events so that we don’t make any unnecessary changes. The following scenarios would not introduce changes to the Palo Alto gateway:

-   Instances that are started but that don’t use the Palo Alto as their internet gateway. For example, there can be multiple internet gateways configured and we're only concerns with instances that use the Palo Alto to reach the internet.
    
-   In instances with multiple interfaces, the script checks all the interfaces, and only includes those that use the Palo Alto instance as an internet gateway.
    
-   Security group rules that have a source from within the AWS VPC (Virtual Private Cloud) will be filtered out. The Palo Alto gateway in this instance is used as an internet gateway, and so traffic from within the VPC would not pass through it.
    
-   Security group rules that reference other security groups as a source will also not be included. These rules imply that the traffic would be local to the VPC and so would not pass the internet gateway.
    

Also before adding a rule, a test is made to make sure traffic is not already allowed, and only after making sure that traffic is denied, we will add a new rule.



**
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### Rule location

**

The code will also only add rules at the bottom so that the security administrator can create rules at the top of the rule base that would override anything added dynamically. This can be used to control the rules automatically added. Furthermore, we specify in the code the bottom most rule that the new rules must  go above in order to avoid adding rules below bottom rules such as the cleanup rule.



**
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### Cleaning up

**

When instances are stopped or rules in security groups are removed we want the rules that we added to be removed. To avoid removing any permanent rules added by the security administrator, the code will only remove rules that it added previously to the rule base based on the naming convention mentioned earlier. The following events are monitored as triggers:

-   StopInstances: An instance was stopped.
    
-   RevokeSecurityGroupIngress: A rule was removed from a security group.
    



**
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### Imported Modules

**

I tried not to import any modules that don’t come with a default installation of Python except when needed. The only exceptions are:



**
----------------------------------------------------------------------------------------------------------------------------------------------

#### Boto3

**

Boto3 is the AWS SDK for python. Using boto3 we can make API calls to AWS to get relevant information that will help us gather the necessary details to read events from AWS and build the rules and changes we want to push to Palo Alto. More details on boto3 [https://boto3.readthedocs.io/en/latest/](https://boto3.readthedocs.io/en/latest/).



**
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

#### Paloalto.py

**

These are functions that I wrote to interface with the Palo Alto gateway. The functions include adding/deleting rules or objects, searching rules and getting details, and saving changes.

To download the latest version of this code, refer to the [github link](https://github.com/akittana/Paloalto-fw). You can also refer to this [blog post](https://securitynik.blogspot.ca/2016/07/writing-palo-alto-firewall-rest-api.html) which goes over the details for writing it.



**
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

#### Netaddr

**

I used the IPAddress and IPNetwork functions from netaddr to allow quick checks on IP addresses (For example, if an IP address belongs to a certain subnet).



**
----------------------------------------------------------------------------------------------------------------------------------------------------------------------

### Event Handling Logic

**

The main function in the code is the lambda\_handler function. When a Lambda trigger occurs, AWS calls this function and passes the event details that triggered it, which in this case would be adding a new entry to S3 by CloudTrail. Four things to take into consideration:

1.  The event passed by AWS contains the location of the S3 object that has the new cloudtrail entries. Our first step is to extract the name and location of this file.
    
2.  Second we have to retrieve the file using the S3 methods from boto3, and then uncompress it using gzip.
    
3.  The contents are then parsed as json to allow us to read and extract properties easily.
    
4.  Finally we iterate through all the records in the logs provided searching for any of the following events:
    



**
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

#### StartInstances

**

1.  Call event\_StartInstances which returns the list of rules relevant to the Instance in the event. In this function, a list of instance Ids are extracted, and the following is performed for each instance.
    

1.  First a list of relevant subnets is created. Relevant subnets are those that use the Palo Alto as their internet gateway.
    
2.  Second, a list of all interfaces belonging to the instance is created along with the subnets each belongs to.
    
3.  For each interface that belongs to a subnet in the relevant subnets, a list of security groups attached to it is compiled.
    
4.  Finally, the rules of all security groups compiled are parsed through, and the relevant rules are added to a list to be sent back.
    

3.  For each rule returned, first we have to convert the format to something that can be understood by Palo Alto. This means that we need to add zone definition, translate the destination port to a corresponding service and application, and specify the action for the rule. I used the aws\_rules\_to\_pa function to convert the format, which in turn uses aws\_to\_pa\_services to map port numbers to application and service combinations.
    
4.  Once the format is changed, we can now test the existing rule base allows this traffic. If it is already allowed, then we move to the next rule in our list from the first Step, otherwise we add the rule on the Palo Alto gateway and move it to the proper location.
    
5.  Finally, commit to save the changes on the Palo Alto.
    



**
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

#### StopInstances

**

1.  Call event\_StopInstances to get a list of Instance Ids from the log event.
    
2.  For each instance id, call paloalto\_rule\_findbyname to get a list of all existing rules added by earlier by our code.
    
3.  Remove each rule returned.
    
4.  Commit to save changes
    



**
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

#### AuthorizeSecurityGroupIngress

**

1.  Call event\_AuthorizeSecurityGroupIngress to get a list of all rules to be added. (Similar function to event\_StartInstances described above).
    
2.  Convert each rule to Palo Alto format using aws\_rules\_to\_pa.
    
3.  Find if there are already existing rules that would allow the traffic for each rule, and discard any rule that has a match.
    
4.  Add remaining rules and move them to the proper location.
    
5.  Commit to save changes.
    



**
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

#### RevokeSecurityGroupIngress

**

1.  Call event\_RevokeSecurityGroupIngress to get a list of relevant security rules to be removed.
    
2.  Find all rules added by our code for this security group (using the rule names)
    
3.  Compare the matching rules on the Palo Alto with the list of relevant rules from Step 1.
    
4.  Remove rules that match both lists.
    
5.  Commit to save changes.
    



**
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

In action
---------

### Setup

**

To run the code as is, the following will be required:

1.  Palo Alto instance configured with a publicly accessible IP address for management.
    
2.  Lambda function created with the following settings (For help configuration the lambda function, refer to this [link](http://docs.aws.amazon.com/lambda/latest/dg/welcome.html), and in particular [Using AWS Lambda with AWS CloudTrail](http://docs.aws.amazon.com/lambda/latest/dg/with-cloudtrail.html)):
    

1.  Handler should be set to lambda.lambda\_handler.
    
2.  No VPC set. (If you would like to set a VPC, refer to the Notes section below for more details).
    
3.  IAM role (with policy attached) to allow the lambda function access to query your S3 and EC2 resources.
    
4.  Timeout value of 25 seconds.
    
5.  Trigger set to the S3 bucket containing the CloudTrail logs.
    
6.  Runtime set to ‘Python 2.7’
    

4.  Finally you will need to upload the code as a zip file to your lambda function. Before doing so, there are some hardcoded variables that need to be set first (All of which are at the top of the lambda\_handler function in lambda.py):
    

1.  pa\_ip: IP address of your Palo Alto gateway.
    
2.  pa\_key: Access key for the Palo Alto gateway (Refer to the [Pan-OS XML API User guide](https://www.paloaltonetworks.com/documentation/71/pan-os/xml-api) for more details on this, and specifically [this page](https://www.paloaltonetworks.com/documentation/71/pan-os/xml-api/get-started-with-the-pan-os-xml-api/get-your-api-key)).
    
3.  pa\_bottom\_rule: Name of the rule which the lambda function would be adding on top of. This would usually be the clean up rule in your security policy.
    
4.  pa\_zone\_untrust: Name of the outside zone configured on the Palo Alto gateway.
    
5.  pa\_zone\_trust: Name of the inside zone configured on the Palo Alto gateway.
    
6.  pa\_sgp: name of security profile group in Palo Alto to be set on rules added.
    
7.  igwId: Instance id of the Palo Alto gateway.
    





**
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### Runtime

**

In the following example, i had a simple setup of a 3 web server instances that use a Palo Alto instance as their internet gateway. I set the variables for my lambda function to point to my Palo Alto, provided the Access Key, etc.

I had a basic security rule base configured with 4 rules initially:

-   Two rules for my web servers. One rule to allow access in (on tcp ports 80,81, and 8000), and one rule to allow access out from the web servers.
    
-   One rule to deny any clear text authentication protocols such as ftp, telnet, etc.
    
-   Finally a clean up rule so that all other dropped traffic is logged.
    



**
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

![base rulebase.png](https://lh6.googleusercontent.com/pRdUR-WNIvHeTV6FjeNMeg29FecvVoKBLUzKl48L60I56rTKuXwQjYAbU8CfxdGelgoPrMQkXk3vhhv_WNxQK5Vz7hmEVn5LC5fD1G0dUnK2SzjxoUPIn07Z8TVHhmRq1qgS4I8g)
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

**

I set the ‘Clean up’ rule to be my bottom rule in the lambda function, so that all rules created would be added between rules 3 and 4.



**
------------------------------------------------------------------------------------------------------------------------------------------------

#### Starting an Instance

**

I then started my 'web server 2' instance which had the IP address 172.20.200.225, and had the webserver\_sg security group assigned, which allowed traffic from any internet source to destination ports 80 and 443:



**
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

![](https://lh6.googleusercontent.com/wr6igXAxe9GhnfcpEtb5pYWJ5QYlYbWkL0S9E0Qw-lFb3n4n042ZGSn6rieCjAVvyTEhiaO60k3zEaxE_pbdYXVtA16IgNvxC_JOdCflzLhO2SHyzKSwKNpsw_1Otpa1tD4Qg5Su)
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

![](https://lh5.googleusercontent.com/9JSwl0-Vf_J8f5c_HVlqwVpK5EB2uXD86HF6cjDpDLxUzoiZgOVl0q6xgag6_VBzeBJMvCKJEWGG2UPiRww0-nYu6U7o0-iIDTUin1ndQpGOjBVaZrsexPKF47LvNwQl5Ni8iVNM)
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

**

Once the instance is started, you can see the Palo Alto rulebase updated with new rule #4:



**
----------------------------------------------------------------------------------------------------

![](https://lh6.googleusercontent.com/OnW1OtnOlbsWL4nD1XtaKgJ2-yFVkwTsIJ0Ww2teYnLxEeuSQrcP0BS49G_8yDUIvlXZ0H3mG1d-Uf1ajq8cr0lrUd7tAz6pVXyfVHKthCfm0jhe7dO3gTryC-_eU8HM8EUCe7gs)
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

**

Note that only one rule was added (for ssl - tcp port 443) since port 80 was already allowed by rule #2 in the rulebase.



**
----------------------------------------------------------------------------------------------------------------------------------

#### Adding rules to a security group

**

Next I updated the security group ‘webserver\_sg’ and added two new rules:



**
------------------------------------------------------------------------------------

![](https://lh5.googleusercontent.com/46Pxeb2ZUQVETf52ymHPQ-03Cji87Lwa3CjjQDYVWzu3Lu8s28jN4s9BlLx54JOJvR_CGorxkXEUCuNt9qs34hd1-wJlS2k8h6LS7ZnL4HQA4dCen3oRl9TpgecHIdPXM4OkYLgK)
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

**

The lambda function adds two new rules with the security group id as the name:



**
----------------------------------------------------------------------------------------

![](https://lh6.googleusercontent.com/Qb54RXqineuFXavm8noxNguU-I4d0APqWpft9whZQltQ7JSb4I7VQ6-fzr4vOd5VC7x89YGZvDTb731U37YnftVFqpWk5PBheoz_QbbnOkNZQ1QJrzBcYxOaGyc0fc2RC6Ppc6qb)
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

#### Removing a rule from a security group

**

Finally, I removed one of the newly added rules from the security group (for port 22):



**
------------------------------------------------------------------------------------------------

![](https://lh3.googleusercontent.com/pq04g6rCrcMBP58fRWD1_O7bMK-EWexRQs7lhZAZsspwVXjEqNq7Nbn_i1zXUsEAuze6SDvTXVv25M38GsxfXegahuk-ez4WveVWV8PBnchCyOOsc6N8JCkz9W0hgzBCWymnqRih)
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

**

And the rulebase was updated accordingly:

![](https://lh3.googleusercontent.com/VTPFGszxJrKYRnvxHm0zW6GTqmGYhBZXxnwE7rGboIqp9nN71OYaUNK_M4oGjzxi-fGVglshKCV8XMuvesaVgbQX_uspYREi-duL6yZYLxiE9eLUojYCKqkmb_dwZPl5HpPV0yxk)



**
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Notes and considerations
------------------------

**

-   There might be a delay from the time of an event to the time the action is seen in the Palo Alto gateway. This is because AWS can have up to 5-15 minutes delay from the time an API call is made to the time it is logged in CloudTrail. I am not aware of an easy way to overcome this other than configuring the lambda function to run on a schedule (for example every 1 to5 minutes,) or moving the code to be run continuously on an instance that has access to CloudTrail and can monitor it in real time.
    
-   In my tests, I used a public IP address of the Palo Alto gateway to configure it. This was easier since I didn’t place my lambda function to run from within my VPC, and so it couldn’t access the private IP address. To have the lambda function access the Palo Alto gateway through a private IP address, the lambda function must be run from within the VPC with security groups assigned to allow it to access the private IP of the Palo Alto. Furthermore, running lambda from within the VPC might interfere with how it accesses S3 objects since those are accessed through the internet. The easiest way to get around this to have an endpoint created in the VPC for accessing S3 (See [https://aws.amazon.com/blogs/aws/new-vpc-endpoint-for-amazon-s3/](https://aws.amazon.com/blogs/aws/new-vpc-endpoint-for-amazon-s3/)).
    
-   All ICMP rules from AWS are treated the same when pushed to Palo Alto (configured with the application ‘icmp’ that would allow all types of ICMP regardless of the configuration in the security groups). Modify the function aws\_to\_pa\_services to introduce more granularity.
    
-   Currently only inbound rules from the security groups are examined and added, but I will be adding support for outbound rule access as well.
    

Download

You can download the latest version of the code [from github](https://github.com/akittana/aws-lambda-paloalto).

To use the code as is, you only need to upload the zip file to your lambda function. If you want to make modifications, you have to zip all files (lambda.py,paloalto.py, netaddr, adn netaddr-0.7.18.dist-info).

I hope this has been helpful, and note that while the functionality described in this post should be fully functional, there are a number of other features that are in progress, and the github link will be updated as these features are completed.

  


**
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

#### [Source](https://www.redblue.team/feeds/4768790912867643560/comments/default)

<br/>
---

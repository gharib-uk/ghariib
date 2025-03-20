---
title: "Using Kafka as a Fast Correlation Engine"
date: 2025-01-04
categories: 
  - "cybersecurity"
  - "security"
tags: 
  - "ai"
  - "cybersecurity"
  - "security"
  - "soc"
  - "software"
---

![](https://socprime.com/wp-content/uploads/Kafka-Streams-400x234.jpg)

In this article, we explore how Kafka Streams can be utilized for filtering and correlating events in real time, effectively transforming Kafka into a high-speed correlation engine. By leveraging the capabilities of **ksqlDB**, you can deploy content rules and filter alerts directly within Kafka. This approach enables real-time filtration and aggregation of log event flows using an intuitive SQL-like language.

## Environment Setup

Our environment includes the following components:

- **Test Kafka Cluster**: A single-node Kafka cluster operating in KRaft mode (Kafka Raft Metadata mode). KRaft replaces the traditional ZooKeeper setup, simplifying Kafka’s architecture and improving operational efficiency.
- **Kafka-UI**: A graphical user interface for managing and monitoring Kafka clusters. It allows easy visualization and manipulation of topics, schemas, and consumer groups.
- **ksqlDB Server**: This component enables real-time stream processing with SQL-like queries. It connects to the Kafka cluster, enabling filtering, transformation, and aggregation of streaming data.
- **ksqlDB CLI**: A command-line interface for interacting with the ksqlDB server. It allows users to write and execute SQL-like queries, define streams, and interact with data programmatically.

Our TEST environment:![](https://socprime.com/wp-content/uploads/kafka-test-environment.png)

## Test Environment Overview

**Data Source**

We use a **“wineventlogs”** topic in Kafka, populated by the **Elastic Winlogbeat agent**, which forwards Windows Event Logs in JSON format.

**Objective**

Filter events based on **SOC Prime TDM content** and save the results to a **“windows\_alerts”** topic for further consumption.

![](https://socprime.com/wp-content/uploads/kafka-topics.png)

For example we would like to filter events based on the SOC Prime TDM content and save message to result “windows\_alerts” topic for further consuming:

![](https://socprime.com/wp-content/uploads/kafka-windows-alerts.png)

## Step-by-Step Implementation

**1\. Connect to the ksqlDB CLI**  
Use the following command to connect to the ksqlDB CLI:

```
podman exec -it ksqldb-cli ksql http://ksqldb-server:8088
```

**2\. List Current Topics**  
To view all topics in your Kafka cluster, use:![](https://socprime.com/wp-content/uploads/Kafka-Cluster.png)

**3\. Examine the Topic Schema**  
Inspect the **wineventlogs** topic to understand its data structure:

```
PRINT wineventlogs;
```

![](https://socprime.com/wp-content/uploads/Kafka-logs.png)

You can also use Kafka-UI to view the messages stored in the topic.

![](https://socprime.com/wp-content/uploads/Kafka-UI.png)

## Defining a Stream Schema

To process data in ksqlDB, you must first define a stream schema that matches the JSON structure of your data. Below is the **CREATE STREAM** statement for the wineventlogs topic:

```
CREATE STREAM WINLOGEVENTS (
	`@timestamp` VARCHAR,
	`@metadata` STRUCT<
    	beat VARCHAR,
    	type VARCHAR,
    	version VARCHAR
	>,
	winlog STRUCT<
    	version INT,
    	event_data STRUCT<
        	ProcessId VARCHAR,
        	CommandLine VARCHAR,
        	SubjectDomainName VARCHAR,
        	MandatoryLabel VARCHAR,
        	SubjectUserName VARCHAR,
        	SubjectLogonId VARCHAR,
        	TargetUserSid VARCHAR,
        	SubjectUserSid VARCHAR,
        	TargetUserName VARCHAR,
        	ParentProcessName VARCHAR,
        	TargetLogonId VARCHAR,
        	NewProcessName VARCHAR,
        	TargetDomainName VARCHAR,
        	TokenElevationType VARCHAR,
        	NewProcessId VARCHAR
    	>,
    	api VARCHAR,
    	task VARCHAR,
    	process STRUCT<
        	pid INT,
        	thread STRUCT<
            	id INT
        	>
    	>,
    	keywords ARRAY<VARCHAR>,
    	provider_guid VARCHAR,
    	opcode VARCHAR,
    	record_id BIGINT,
    	computer_name VARCHAR,
    	event_id VARCHAR,
    	provider_name VARCHAR,
    	channel VARCHAR
	>,
	event STRUCT<
    	created VARCHAR,
    	code VARCHAR,
    	kind VARCHAR,
    	provider VARCHAR,
    	outcome VARCHAR,
    	action VARCHAR
	>,
	log STRUCT<
    	level VARCHAR
	>,
	message VARCHAR,
	host STRUCT<
    	name VARCHAR,
    	hostname VARCHAR,
    	architecture VARCHAR,
    	os STRUCT<
        	version VARCHAR,
        	family VARCHAR,
        	name VARCHAR,
        	kernel VARCHAR,
        	build VARCHAR,
        	type VARCHAR,
        	platform VARCHAR
    	>,
    	id VARCHAR,
    	ip ARRAY<VARCHAR>,
    	mac ARRAY<VARCHAR>
	>,
	ecs STRUCT<
    	version VARCHAR
	>,
	agent STRUCT<
    	name VARCHAR,
    	type VARCHAR,
    	version VARCHAR,
    	ephemeral_id VARCHAR,
    	id VARCHAR
	>
) WITH (
	KAFKA_TOPIC = 'wineventlogs',
	VALUE_FORMAT = 'JSON'
);
```

So, stream successfully created:

![](https://socprime.com/wp-content/uploads/Kafka-stream.png)

![](https://socprime.com/wp-content/uploads/Kafka-stream-2.png)

Lets test our WINLOGEVENTS stream, filter the stream as new data arrives:

![](https://socprime.com/wp-content/uploads/Kafka_winlogevents.png)

So we see, that our stream works properly and we see all throughput messages.

## Creating a Filtered Stream

**Create a New Kafka Topic**  
First, create a topic for filtered messages windows\_alerts:

![](https://socprime.com/wp-content/uploads/New-Kafka-Topic.png)

**Define a New Stream for Alerts**  
Filter events using the SOC Prime TDM rule **Possible Antivirus or Firewall Software Enumeration (via process\_creation)**. Below is the **CREATE STREAM** statement:

- CommandLine  according to data will be mapped to winlog->event\_data->CommandLine
- ParentImage according to data will be mapped to  winlog->event\_data->ParentProcessName
- Host to host->hostname

```
CREATE STREAM WINDOWS_ALERTS WITH (KAFKA_TOPIC = 'windows_alerts') AS SELECT host->hostname AS Host, winlog->event_data->CommandLine AS CommandLine, winlog->event_data->ParentProcessName AS ParentImage FROM WINLOGEVENTS WHERE ( (winlog->event_data->CommandLine LIKE '%AntiSpywareProduct%' OR winlog->event_data->CommandLine LIKE '%AntiVirusProduct%' OR winlog->event_data->CommandLine LIKE '%FirewallProduct%') AND NOT (winlog->event_data->ParentProcessName LIKE '%phpstorm64.exe' OR winlog->event_data->ParentProcessName LIKE '%pycharm64.exe') AND winlog->event_data->CommandLine LIKE '%displayName%' AND winlog->event_data->CommandLine LIKE '%productState%') OR ( winlog->event_data->CommandLine LIKE '%Win32_Process%' AND winlog->event_data->CommandLine LIKE '%AvastUI.exe%' AND winlog->event_data->CommandLine LIKE '%AvastSvc.exe%' AND winlog->event_data->CommandLine LIKE '%FortiWF.exe%' AND winlog->event_data->CommandLine LIKE '%xagt.exe%' AND winlog->event_data->CommandLine LIKE '%fcappdb.exe%' );
```

So we defined the stream with filtering by our rule, lets test it!

## Testing the Filtered Stream

**P****roduce a Test Message**  
Publish a test message containing “fcappdb.exe” in the CommandLine field to the wineventlogs topic.

![](https://socprime.com/wp-content/uploads/Test-message_Kafka.png)

**Check the Filtered Stream**  
Inspect the WINDOWS\_ALERTS stream:  
SELECT \* FROM WINDOWS\_ALERTS EMIT CHANGES;

![](https://socprime.com/wp-content/uploads/Check-filtered-stream_Kafka_1.png)

And we see than Alert is here, cool!

And check the topic- we see the result correlated message here:![](https://socprime.com/wp-content/uploads/Check-filtered-stream_Kafka_2.png)

Thats it!

## **Conclusion**

Using Kafka Streams with ksqlDB, you can build a high-performance correlation engine (like in SIEM). This approach allows for real-time event filtering, reducing reliance on traditional SIEM solutions. Additionally, by integrating content from **SOC Prime TDM**, you can enrich your correlation logic to detect sophisticated threats efficiently.

  
  

The post Using Kafka as a Fast Correlation Engine appeared first on SOC Prime.

Go to Source

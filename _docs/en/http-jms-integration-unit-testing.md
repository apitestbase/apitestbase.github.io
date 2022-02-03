---
title: HTTP-JMS Integration Unit Testing
permalink: /docs/en/http-jms-integration-unit-testing
key: docs-http-jms-integration-unit-testing
---
## Introduction To The API Under Test
Suppose you have a Data Ingestion API that exposes an HTTP endpoint. When a client invokes it with data, it delivers the data, unchanged, to a JMS (Solace) queue.

![Data Ingestion API](../../screenshots/http-jms/data-ingestion-api.png)

## Test Isolation
### Use Stub Queue Endpoint
To integration unit test the Data Ingestion API, design the API to point to different dependencies in different environments. This is normally achieved by setting different dependency endpoint addresses in different property files. Each property file contains all properties for the API for a specific environment like Dev/Test/QA/Prod. When deploying and running the API, dynamically load the property file for that specific environment.

In an integration unit testing environment like the Dev environment (which is typically a developer's local machine), a property file like `data-ingestion-api-dev.properties` is used for the Data Ingestion API. The property file contains something like below
~~~
    jms.solace.jndi.provider.url=smf://localhost:55555
    jms.solace.connection.factory.jndi.name=JNDI/CF/default
    jms.solace.vpn=default
    jms.solace.queue.jndi.name=/jndi/q/data/ingestion/api/stub/out
~~~ 
Here the JMS properties are of a local JMS (Solace) stub queue. We use a local dedicated stub queue, instead of any fully implemented and shared JMS (Solace) queue like one in the SIT environment, as the dependency during integration unit testing, like shown below.

![Data Ingestion API Test Isolation](../../screenshots/http-jms/data-ingestion-api-test-isolation.png)

### Create Stub Queue
To create the stub queue, you can simply do it via the Solace web console.
* Create a physical queue, like `q/data/ingestion/api/stub/out`. Use default settings for simplicity.
* And then create the JNDI queue `/jndi/q/data/ingestion/api/stub/out` which is effectively an alias, and associate it with the physical queue.

## Test Case Creation
It is recommended that you have a look at [Quick Start](/docs/en/quick-start) if ATB is new to you.

Create a test case `Positive` under a folder for the Data Ingestion API, with below test steps
```
    1. (JMS step) Setup - clear stub output queue
    2. (HTTP step) Invoke the API to ingest data
    3. (Wait step) Wait for 0.5 seconds
    4. (JMS step) Check stub output queue depth equals 1
    5. (JMS step) Browse the stub output queue and assert message body   
```
Step 3 might be optional depending on the Data Ingestion API's implementation. It is needed when the implementation is of asynchronous style, i.e. the API returns no matter whether the JMS message delivery is acknowledged. 

Run the test case by clicking the `Run` button, and check the test report.

## Sample Test Case
The test case created above is available for download at <a href="../../sample-testcases/http-jms/Positive.json" download>sample test case</a>. After download, right click any folder on ATB UI and import it.

## What is Integration Unit Testing?
Refer to [this post](https://medium.com/@zhengwang666/integration-unit-testing-683fbf995c43).
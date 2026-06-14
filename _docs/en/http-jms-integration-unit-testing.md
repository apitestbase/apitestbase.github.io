---
title: HTTP-JMS Integration Unit Testing
permalink: /docs/en/http-jms-integration-unit-testing
key: docs-http-jms-integration-unit-testing
---
## Introduction To The API Under Test
Suppose you have a Data Ingestion API that exposes an HTTP endpoint. When a client invokes it with data, it delivers the data, unchanged, to a JMS (Solace) queue.

![Data Ingestion API](../../screenshots/http-jms/data-ingestion-api.png)

## Test Isolation
To integration unit test the Data Ingestion API, you would typically set up and use a local Solace queue. This has some benefits

* The test includes testing the side effect of the Data Ingestion API. That is, the API interacts with a real Solace queue, and actually results in the message being persisted in the queue.
* The test is fully controlled by you on your local machine, without impacting any other developer.

![Data Ingestion API Test Isolation](../../screenshots/http-jms/data-ingestion-api-test-isolation.png)

### Set up the Stub Solace Instance
The simplest way of spinning up a local Solace instance is probably launching a Docker container.

This can be done through ATB's Docker test step.

### Create the Stub Queue
To create the stub queue, we can call the Solace SEMP v2 API from ATB's HTTP test step.

### Use the Stub Queue
Design the Data Ingestion API to point to different dependencies in different environments. This is normally achieved by setting different dependency endpoint addresses in different property files. Each property file contains all properties for the API for a specific environment like Dev/Test/QA/Prod.

When deploying and running the API, dynamically load the property file for that specific environment.

Here is a sample property file `data-ingestion-api-dev.properties` for a developer's local machine.
~~~
solace.host=localhost:55555
solace.vpn=default
solace.queue.name=q/data/ingestion/api/stub/out
~~~

## Test Case Creation
Create a test case `Positive` under a folder for the Data Ingestion API, with below test steps

```
1. (Docker step) Setup - Start solace-auto
2. (HTTP step) Setup - Ensure Solace is ready
3. (HTTP step) Setup - Delete stub output queue
4. (HTTP step) Setup - Create stub output queue
5. (HTTP step) Invoke the API to ingest data
6. (Wait step) Wait for 0.5 seconds
7. (JMS step) Check stub output queue depth equals 1
8. (JMS step) Browse the stub output queue and assert message body
```

Step 6 might be optional depending on the Data Ingestion API's implementation. It is needed when the implementation is of asynchronous style, i.e. the API returns no matter whether the JMS message delivery is acknowledged.

## Sample Test Case
The test case created above is available for download at <a href="../../sample-testcases/http-jms/Positive.json" download>sample test case</a>.

After download, right click anywhere in the left side pane on ATB UI, and select `Import Test Case` to import it.

## Further Note
Here we are setting up the stub Solace and queue in the test case, for making the example simple. In real work, we don't want to duplicate the test setup steps here and there in many test cases. Instead, we want to manage test setup in a structured manner. Refer to [Structured Test Setup for Integration Unit Test Isolation](/docs/en/structured-test-setup-for-integration-unit-test-isolation) for more details.
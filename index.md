---
title: Home
layout: article
permalink: /
key: home
pageview: true
show_title: false
tags:     # Not sure why. Unlike other article pages, here can't use 'tags: tag1 tag2, ...', because it will cause 'tags[0]' in _includes/article-info.html not working, hence not rendering the 'keywords' metadata.
  - home
  - integration-unit-testing
  - integration-testing
  - test-automation 
  - api-testing
  - database-testing
  - http-stub
  - http-testing
  - soap-testing
  - mq-testing
  - ace-testing
  - mock-service
  - data-driven-testing
  - amqp-testing
  - file-testing
  - ftp-testing
  - sftp-testing
  - activemq-testing
  - solace-testing
  - mqtt-testing
  - jms-testing
---
# Test what your API *really did*

Most API testing tools only verify the **response**.

But real APIs do much more. They may:

-   write records to **databases**
-   publish messages to **queues**
-   upload **files**
-   trigger **integration flows**
-   call **downstream services**

If your test only checks the response, you may miss what the API actually changed.

**API Test Base** allows you to verify both the response **and the side effects** of an API in one automated test.

<div style="text-align:center"><a class="button button--outline-primary button--pill" href="/docs/en/quick-start">Quick Start</a></div>

------------------------------------------------------------------------

# Test APIs and their dependencies – together

Modern APIs rarely work alone.

They interact with databases, message queues, files, and other services.

Testing an API should not stop at the response.

Example:

Send request:

    POST /orders

Assertions:

    ✓ HTTP response = 200

    ✓ Order record created in database
    ✓ Message sent to queue
    ✓ File uploaded to FTP server
    ✓ Downstream API called successfully

API Test Base makes it easy to verify these behaviors in a single automated test case.

------------------------------------------------------------------------

# Test APIs across different system interfaces

In many systems, APIs are not limited to HTTP endpoints.

They may be exposed through different system interfaces such as:

- HTTP / REST
- SOAP
- Message queues (JMS, AMQP, MQTT, IBM MQ)
- Databases
- File systems
- FTP / SFTP

For example:

- an HTTP API may update database records
- a message sent to a queue may trigger an integration flow
- a file drop may start a processing pipeline

API Test Base allows creating tests that interact with these interfaces and verify their effects across the system.

------------------------------------------------------------------------

# Built‑in test setup for integration testing

Integration tests often require preparing test data before calling an API.

For example, when testing an API that updates a database record, a test may need to:

1.  clear a table
2.  insert a test record
3.  call the API
4.  verify the updated data

With many tools this requires external scripts.

API Test Base allows performing these steps directly inside the test case.

Example workflow:

Database step\
→ clear table and insert test record

HTTP step\
→ call REST API

Database step\
→ query table and assert updated values

This makes it easier to create **self‑contained integration tests**.

------------------------------------------------------------------------

# Feature highlights

-   Support many protocols --- not just HTTP
-   No‑code / low‑code test case creation
-   Standalone requests and structured test cases
-   Built‑in data‑driven testing
-   HTTP stubs (mock servers)
-   Pattern‑based test case generation
-   Automated test setup
-   Docker support
-   Git‑based team collaboration
-   Offline‑first --- all data stored locally

------------------------------------------------------------------------

# User Interface

![UI Glance](../../screenshots/ui-glance.png)

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

API Test Base is a free, offline‑first tool for testing APIs **and their side effects** — the records your API writes to a database, the messages it publishes to a queue, the downstream services it calls.

No account, no cloud. You download it and run it on your own machine or in your CI/CD pipeline. Test cases are created with a **no‑code / low‑code** UI, so you can be productive in minutes.

<div style="text-align:center">
  <a class="button button--primary button--pill" style="margin: 0.25rem 0.4rem;" href="/docs/en/quick-start">Download</a>
  <a class="button button--outline-primary button--pill" style="margin: 0.25rem 0.4rem;" href="/docs/en/creating-an-automated-test-case">See how it works</a>
</div>

<div style="text-align:center; margin-top: 1rem;">
  <img alt="API Test Base test case run result — three steps (set up database data, invoke the HTTP API, check the database) all passed, shown in green with per-step timings" src="../../screenshots/basic-use/test-case-run-result.png" width="90%">
</div>

------------------------------------------------------------------------

## Verify the side effects, not just the response

Tools like **Postman** and **Bruno** make it easy to verify an API's **response** — and that's where most testing stops. But real APIs do more: they write to databases, publish messages to queues, and call downstream services. If your test only checks the response, you may miss what the API actually changed.

For example, a single `POST /orders` call can be verified end to end:

    ✓ HTTP response = 200
    ✓ Order record created in database
    ✓ Message sent to queue
    ✓ Downstream API called successfully

API Test Base verifies both the response **and** these side effects in one automated test case. See [how API Test Base compares to Postman and Bruno](/comparison).

------------------------------------------------------------------------

## Test APIs triggered by queue messages and file drops

Not every API is an HTTP endpoint. The same business logic may be triggered by:

- a message on a queue — [IBM MQ](/docs/en/mq-request), [JMS](/docs/en/jms-request), [AMQP](/docs/en/amqp-request), or [MQTT](/docs/en/mqtt-request)
- a file dropped on an [FTP](/docs/en/ftp-request) or [SFTP](/docs/en/sftp-request) server
- a SOAP call

API Test Base can send the trigger over the right protocol and then verify the outcome — so you can test the API the same way no matter how it is invoked. That makes it well suited to integration and ESB scenarios such as IBM MQ testing, JMS testing, SFTP testing, and SOAP API testing.

------------------------------------------------------------------------

## Built on a method: Integration Unit Testing

API Test Base is built around **Integration Unit Testing** — testing an API as a black box, in isolation, by testing both its **contract** (request and response) and its **side effects** (database records, messages, files, downstream calls).

Because the tests know nothing about the API's internal code, they stay stable as the implementation changes. The same API tests you run by hand during development can be [automated in a CI/CD pipeline](/docs/en/integration-unit-testing-automation-in-cicd-pipeline) to run the whole set on every build.

[Learn more about Integration Unit Testing →](/docs/en/what-is-integration-unit-testing)

------------------------------------------------------------------------

## Built‑in test setup for integration testing

Integration tests need the right state before the API runs, and with most tools that means external scripts. API Test Base runs the setup directly inside the test. A typical database test is three steps in one case:

Database step\
→ clear the table and insert a test record

HTTP step\
→ call the REST API

Database step\
→ query the table and assert the updated values

Setup isn't limited to seeding data. For full isolation, a setup can stand up a dedicated **stub database** — a real database such as PostgreSQL, running in a throwaway Docker container — and create its schema and tables, so the API runs against state that's entirely the test's own. [Structured test setup](/docs/en/structured-test-setup-for-integration-unit-test-isolation) lets you define each setup once, reuse it across folders, and run only what's needed.

------------------------------------------------------------------------

## Feature highlights

-   Many protocols --- not just HTTP
-   [No‑code / low‑code test case creation](/docs/en/creating-an-automated-test-case)
-   Standalone requests and structured test cases
-   Built‑in [data‑driven testing](/docs/en/data-driven-testing)
-   [HTTP stubs (mock servers)](/docs/en/http-stubs)
-   Pattern‑based test case generation
-   [Automated test setup](/docs/en/structured-test-setup-for-integration-unit-test-isolation)
-   [Docker support](/docs/en/quick-start)
-   [Git‑based team collaboration](/docs/en/team-collaboration)
-   Offline‑first --- all data stored locally

------------------------------------------------------------------------

## Ready to get started?

Download API Test Base and write your first test in minutes --- then run the same tests in your CI/CD pipeline.

<div style="text-align:center">
  <a class="button button--primary button--pill" style="margin: 0.25rem 0.4rem;" href="/docs/en/quick-start">Download</a>
  <a class="button button--outline-primary button--pill" style="margin: 0.25rem 0.4rem;" href="/docs/en/integration-unit-testing-automation-in-cicd-pipeline">Automate in CI/CD</a>
</div>
---
title: What is Integration Unit Testing?
permalink: /docs/en/what-is-integration-unit-testing
key: docs-what-is-integration-unit-testing
---
**Integration Unit Testing** is a method for functionally testing an API as a black box, in an isolated manner.

The method includes:

* **Testing the API's contract**, i.e. request and response.
* **Testing the API's side effect**, i.e. database record updated, other APIs called, messages sent to a queue, logs written to a file, etc.

'Unit' here refers to API-level test isolation: the API under test runs against stub dependencies — HTTP stubs, a stub database, stub queues, etc. — instead of shared systems. The test therefore fully controls the API's outside world, so every run starts from a known state and behaves deterministically.

Integration unit tests are not coupled with, hence have no knowledge about, the API's internal implementation code. The tests care about the results the API produces in its outside world instead: its response, and the actual outcomes of its side effects, like database data really being changed. Because of this, the tests stay stable as the implementation code is refactored, and one set of tests covers what would otherwise be duplicated across traditional unit tests and integration tests.

For the full reasoning behind the method, see [this post](https://medium.com/@zhengwang666/integration-unit-testing-683fbf995c43){:target="_blank"}.

## Integration Unit
Besides test isolation, 'unit' in the name also has an architectural meaning: we can think of an API as an integration unit. An API has the following characteristics:

* It receives a request message from its client, sends a response message to the client and/or interacts with other systems (database, message broker, FTP server, etc.) or APIs.
* It can use any input protocol, including but not limited to HTTP. RESTful API is the most widely adopted form of API.
* It can be implemented in any programming language, framework or platform.
  * Examples: a Spring Boot REST API, a Next.js REST API, a Django REST API, a Java gRPC API, a MuleSoft message flow, a webMethods service, a StreamSets data pipeline, etc.

When zooming out to a higher level architecture (web app, microservices, event driven, API topology, SOA, etc.), there could be one or more APIs working together to integrate a system (UI, application, database, FTP server, mainframe, etc.) with other systems. Therefore, **an API is a unit of the integration**.

## In Practice
Integration unit tests are run by hand during API development, and automated in a [CI/CD pipeline](/docs/en/integration-unit-testing-automation-in-cicd-pipeline) to run the whole set on every build.

To see the method in practice, start with [HTTP-HTTP Integration Unit Testing](/docs/en/http-http-integration-unit-testing) or [HTTP-Database Integration Unit Testing](/docs/en/http-database-integration-unit-testing).
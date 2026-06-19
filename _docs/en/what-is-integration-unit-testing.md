---
title: What is Integration Unit Testing?
permalink: /docs/en/what-is-integration-unit-testing
key: docs-what-is-integration-unit-testing
---
**Integration Unit Testing** is a method for functionally testing an API as a black box, in an isolated manner.

The method includes

* **Testing the API's contract**, i.e. request and response.
* **Testing the API's side effect**, i.e. database record updated, other APIs called, messages sent to a queue, logs written to a file, etc.

Integration unit tests are not coupled with, hence have no knowledge about, the API's internal implementation code. The tests care about the API's interactions with its outside world instead.

The purpose of integration unit testing is to make testing the API's business logic (functionalities) easier and a better experience for developers.

For more details, please refer to [this post](https://medium.com/@zhengwang666/integration-unit-testing-683fbf995c43){:target="_blank"}.

Integration unit tests are run by hand during API development, and automated in a [CI/CD pipeline](/docs/en/integration-unit-testing-automation-in-cicd-pipeline) to run the whole set on every build.

### Integration Unit
We can think of an API as an integration unit. An API has the following characteristics:

* It receives a request message from its client, sends a response message to the client and/or interacts with other systems (database, message broker, FTP server, etc.) or APIs.
* It can use any input protocol, including but not limited to HTTP. RESTful API is the most widely adopted form of API.
* It can be implemented in any programming language, framework or platform.
  * Examples: a Spring Boot REST API, a NextJS REST API, a Django REST API, a Java gRPC API, a MuleSoft message flow, a webMethods service, a StreamSets data pipeline, etc.

When zooming out to a higher level architecture (web app, microservices, event driven, API topology, SOA, etc.), there could be one or more APIs working together to integrate a system (UI, application, database, FTP server, main frame, etc.) with other systems. Therefore, **an API is a unit of the integration**.
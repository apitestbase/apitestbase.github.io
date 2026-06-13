---
title: What is Integration Unit Testing?
permalink: /docs/en/what-is-integration-unit-testing
key: docs-what-is-integration-unit-testing
---
**Integration Unit Testing** is a method for functionally testing an API as a black box, in an isolated manner.

The method includes

* **Testing the API's contract**, i.e. request and response.
* **Testing the API's side effect**, i.e. database record updated, other APIs called, messages sent to a queue, logs written to a file, etc.

The API can use any input protocol, including but not limited to HTTP. RESTful API is the most widely adopted form of API.

Integration unit tests are not coupled with, hence have no knowledge about, the API's internal implementation code. The tests care about the API's interactions with the outside world instead.

The purpose of integration unit testing is to make testing the API's business logic (functionalities) easier and a better experience for developers.

For more details, please refer to [this post](https://medium.com/@zhengwang666/integration-unit-testing-683fbf995c43){:target="_blank"}.

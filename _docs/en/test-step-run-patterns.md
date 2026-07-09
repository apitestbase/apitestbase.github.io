---
title: Test Step Run Patterns
permalink: /docs/en/test-step-run-patterns
key: docs-test-step-run-patterns
---
A test step's run pattern controls how the step runs as part of a test case run. To set it, open the test step's details view and go to the **Run Pattern** tab, where a pattern can be chosen and its parameters specified.

There are three patterns:

* **Run once** — the default. The test step runs once.
* **Repeat until pass** — the test step is run repeatedly until it passes or the **Timeout (ms)** is reached, in which latter case the test step fails. The **Wait between repeat runs (ms)** field specifies the time between the finish of one repeat run and the start of the next.
* **Repeat fixed number of times** — the test step runs the number of times specified in the **Repeat times** field, regardless of whether individual repeat runs pass or fail. After all repeat runs finish, the test step's result is evaluated: the step passes only if every repeat run passes; if any repeat run fails, the step fails.

During the repeat runs of either repeat pattern, the implicit property [`${test.step.repeat.run.index}`](/docs/en/properties#teststeprepeatrunindex) holds the index of the current repeat run, starting from 1.

For examples of **Repeat until pass** in practice — waiting for a dependency to become ready, and tolerating an asynchronous API implementation when verifying its **side effect** — see [HTTP-Database Integration Unit Testing](/docs/en/http-database-integration-unit-testing) and [HTTP-JMS Integration Unit Testing](/docs/en/http-jms-integration-unit-testing).
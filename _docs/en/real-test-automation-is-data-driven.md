---
title: Real Test Automation is Data-Driven
permalink: /docs/en/real-test-automation-is-data-driven
key: docs-real-test-automation-is-data-driven
---
In real-world test automation, test logic tends to stay stable while test data keeps evolving.

A test case can be run once, driven by [custom properties](/docs/en/properties#user-defined-properties), or run one or more times, driven by a [data table](/docs/en/data-driven-testing).

The common thing here is that after test steps development is finished, the user no longer needs to care about them. The user's focus can be narrowed down to the test data alone, freeing mental effort.

In a test case's long active lifecycle, the user seldom needs to change its test steps, but needs to change test data more often.

This is effectively treating the test case as a black box, and test data as its interface/parameters. In this sense, the test case is data-driven, whether the data comes from custom properties or a data table.
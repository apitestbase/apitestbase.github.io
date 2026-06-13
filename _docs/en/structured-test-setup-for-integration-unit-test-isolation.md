---
title: Structured Test Setup for Integration Unit Test Isolation
permalink: /docs/en/structured-test-setup-for-integration-unit-test-isolation
key: docs-structured-test-setup-for-integration-unit-test-isolation
---
One of the core requirements of integration unit testing is to **isolate the API under test**. This means we need to **set up mock/stub dependencies for the API**, to be able to control the testing behavior.

We need an approach that enables **automated test setup** during API development and also inside CI/CD pipeline. The approach ATB recommends is **Structured Test Setup** which has below simple principles:
* Enable running only necessary setups for a test case or folder.
* Enable reusing test setup steps.

The approach makes test setup management and automation simple.

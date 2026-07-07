---
title: Avoid Test Duplication
permalink: /docs/en/avoid-test-duplication
key: docs-avoid-test-duplication
---
If your system maps (perfectly or partially) to the [Test Pyramid](https://martinfowler.com/bliki/TestPyramid.html), you might find test duplication between service tests (API tests) and unit tests (tests of units that constitute API implementation).

API Test Base intends to make service tests — in the form of [integration unit tests](/docs/en/what-is-integration-unit-testing) — fast, reliable, and cheap to modify, helping make many unit tests unnecessary. Refer to Note 2 of the Test Pyramid article. Citing the key part: **If my high level tests are fast, reliable, and cheap to modify - then lower-level tests aren't needed.**

For a complicated internal component, service tests might not be able to cover it well. In this case, unit tests are still needed.
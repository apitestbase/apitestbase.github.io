---
title: Avoid Test Duplication
permalink: /docs/en/avoid-test-duplication
key: docs-avoid-test-duplication
---
If your system can map (perfectly or partially) to the [Test Pyramid](https://martinfowler.com/bliki/TestPyramid.html), you might find test duplication between service tests (API tests) and unit tests (tests of units that constitute API implementation).

API Test Base intends to make service tests fast, reliable, and cheap to modify, so as to help make unit tests unnecessary. Refer to Note 2 of [Test Pyramid](https://martinfowler.com/bliki/TestPyramid.html).
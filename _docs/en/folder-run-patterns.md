---
title: Folder Run Patterns
permalink: /docs/en/folder-run-patterns
key: docs-folder-run-patterns
---
To run a folder in ATB, open the folder's details view and go to the **Run** tab. The **Pattern** field there controls what the run includes — which setups run, and whether test cases run with them — and the **Run** button starts it.

A folder contributes setups in two ways, both under its **Setup** tab. The **Steps** tab holds setup steps defined directly on the folder. The **References** tab holds **Setup References** to setup test cases defined elsewhere; each reference points to one test case, so referencing several test cases means adding several references. Together, a folder's steps and references are its *own setups*; within the folder, the references always run before the steps. Its *ancestor setups* are the own setups of the folders above it, up to the workspace; its *descendants* are the sub-folders beneath it — each with its own setups and test cases — recursively.

There are three patterns:

* **Setups to Here** — runs every setup defined or referenced from the workspace level down to the folder, and no test cases. It prepares the environment without running any test.
* **Default** — runs the folder's own setups, then recurses through everything beneath it: each descendant sub-folder's setups and all test cases. It does not run ancestor setups.
* **All** — the same as Default, plus the ancestor setups first, from the workspace down: ancestor + own + descendants, a fully self-contained run.

| Pattern | Ancestor setups | Folder's own setups | Descendant setups | Test cases |
|---|:---:|:---:|:---:|:---:|
| Setups to Here | ✓ | ✓ | — | — |
| Default | — | ✓ | ✓ | ✓ |
| All | ✓ | ✓ | ✓ | ✓ |

For worked examples of choosing a pattern — during API development, where a run is triggered from the ATB UI, and in a CI/CD pipeline, where it is triggered over ATB's REST API — see [Structured Test Setup for Integration Unit Test Isolation](/docs/en/structured-test-setup-for-integration-unit-test-isolation) and [Integration Unit Testing Automation in CI/CD Pipeline](/docs/en/integration-unit-testing-automation-in-cicd-pipeline).

## A Current Limitation

Running a single test case on its own runs **no** folder setups — neither its own folder's nor its ancestors'. To prepare those setups, run the containing folder (or an ancestor) with **Setups to Here** first, then run the test case.
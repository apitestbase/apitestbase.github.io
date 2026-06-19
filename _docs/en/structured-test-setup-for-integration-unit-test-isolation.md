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

## How It Works

ATB delivers these two principles through two mechanisms:

* **Inheritance and references, to reuse setup steps.** A setup defined on a folder is inherited by the sub-folders beneath it: written once on the parent, it runs ahead of each descendant sub-folder's own setups and test cases when the parent folder is run, instead of being copied into every sub-folder. When folders in different parts of the tree — with no shared ancestor — need the same setup, a folder can add a **Setup Reference** pointing at a setup test case defined elsewhere, reusing those steps without copying them.
* **Run Patterns, to run only what's needed.** A folder's **Run Pattern** decides what a run does: prepare the environment by running setups without any test, or run setups together with the test cases beneath the folder.

Isolation is the goal these serve. To test an API in isolation, it runs against a local, dedicated **stub database** — not a shared database such as one in a SIT environment — so the dependency is fully under the test's control. Creating that stub database and its tables is part of the test isolation work, and it's exactly the kind of prerequisite Structured Test Setup is designed to manage: defined once, reused where shared, and run only when needed.

## The Example

A small "Online Store" application with three REST APIs. To test each API in isolation, the setup stands up a local, dedicated stub database — a PostgreSQL instance in a throwaway Docker container — in place of a shared database, so every run starts from a known, fully controlled state.

| API | Endpoint | Table | Operation |
|---|---|---|---|
| User Preferences API | `PUT /api/users/{userId}/preferences` | `user_preferences` | Insert (new user) or update (existing user) |
| Create Order API | `POST /api/orders` | `orders` | Insert (new order, status `PENDING`) |
| Update Order Status API | `PATCH /api/orders/{orderId}/status` | `orders` | Update (e.g. to `PAID`, `SHIPPED`, `CANCELLED`) |

`user_preferences` is used only by the first API. `orders` is shared by the second and third.

## Folder and Setup Structure

```
[Workspace] Online Store
├── [Folder] Common Test Setups
│   └── [Test Case] Create Orders Table
│
├── [Folder] REST API Tests
│   ├── [Folder Setup]
│   │     [Docker Step] Start PostgreSQL container
│   │     [Database Step] Create "online_store" schema
│   │
│   ├── [Folder] User Preferences API
│   │   ├── [Folder Setup]
│   │   │     [Database Step] Create "user_preferences" table
│   │   ├── [Test Case] Create preferences for new user (insert path)
│   │   ├── [Test Case] Update preferences for existing user (update path)
│   │   └── [Test Case] Invalid theme value returns 400
│   │
│   ├── [Folder] Create Order API
│   │   ├── [Folder Setup Reference] → Common Test Setups / [Test Case] Create Orders Table
│   │   ├── [Test Case] Create order successfully
│   │   └── [Test Case] Invalid amount returns 400
│   │
│   └── [Folder] Update Order Status API
│       ├── [Folder Setup Reference] → Common Test Setups / [Test Case] Create Orders Table
│       ├── [Test Case] Update status to PAID
│       ├── [Test Case] Invalid status transition returns 409
│       └── [Test Case] Update non-existent order returns 404
│
└── [Folder] Third-Party Integration Tests
      (e.g. payment gateway, shipping provider, email/SMS notifications)
```

That's the whole picture: one Docker + schema setup at the top of the REST API tests, a table setup local to the one API that owns its table, and a shared table setup referenced by the two APIs that share a table.

## Why the Setups Live Where They Do

The tree shows *what* the setups are; the placement decisions are what make it work:

* **The stub database is provisioned at the top.** The Docker step (PostgreSQL container) and schema creation sit on **REST API Tests**, so every API beneath it shares one isolated stub database instead of each standing up its own.
* **A table's setup is owned by whoever uses it.** `user_preferences` is created on the **User Preferences API** folder, because that's the only API that touches it. `orders` is used by two APIs, so its setup — **Create Orders Table** — is *referenced* by both the Create Order and Update Order Status folders rather than duplicated in each.
* **A reference can reach anywhere in the tree.** **Create Orders Table** deliberately lives in **Common Test Setups**, outside REST API Tests, to show a Setup Reference isn't limited to a folder's own subtree. The setup itself is written to be repeatable — it drops the `orders` table before creating it — so running it more than once does no harm.

The workspace holds only the application's API tests; the **Third-Party Integration Tests** folder is shown purely to place the REST API tests within the application's full test suite.

## Running the Tests

How a run behaves depends on the folder's [**Run Pattern**](/docs/en/folder-run-patterns) — which setups run, and whether test cases run with them. Two workflows put the patterns to use.

### During API development

While building an API, the developer launches and debugs the implementation in their IDE and uses ATB test cases to verify each change. They run the API folder once with **Setups to Here** to stand up the stub database and create the tables — for Create Order API, that's the REST API Tests folder setup (Docker + schema) plus the folder's own "Create Orders Table" reference. It runs no test cases.

From there they iterate quickly. Running a single test case re-runs no setup at all. Running the API folder with **Default** runs the folder's own setups and all of its test cases, but not the ancestor setups (the Docker container and schema) — so the expensive container start-up isn't repeated between iterations. When a setup itself changes, the developer re-runs **Setups to Here** to apply it before continuing.

### In CI/CD

A pipeline runs a folder with the **All** pattern, so the run is self-contained: the ancestor setups run first, then the folder's own setups and every test case beneath it. See [Integration Unit Testing Automation in CI/CD Pipeline](/docs/en/integration-unit-testing-automation-in-cicd-pipeline) for how to drive this from a pipeline.

Pointing this at a single API folder (say, Create Order API) runs just that API's setups and tests — useful when each API has its own Git repository, so a change to one API doesn't trigger the whole suite. Pointing it at the REST API Tests folder runs everything, for a full pass across all APIs.

One note for the full run: setup deduplication isn't implemented yet, so "Create Orders Table" runs once for each folder that references it — twice across Create Order API and Update Order Status API. Because the setup drops and recreates the table, the repeat is harmless.

## Why This Matters

Each setup is defined once, in the place that matches its scope: on a folder when it belongs only to that folder's subtree, or in a setup test case that other folders reference when it's shared. Nothing is copied, so every prerequisite has a single source of truth. Run together, these setups stand up a stub database that's entirely the tests' own — so the API runs in isolation and its real side effects stay verifiable, which is what ATB exists to do.
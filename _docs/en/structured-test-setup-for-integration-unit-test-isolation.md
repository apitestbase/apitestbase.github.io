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

## Walking Through the Setups

**Online Store (workspace)** — holds the API tests for this application. The Third-Party Integration Tests folder is shown only to place the REST API tests in the context of the application's full test suite; its setup is outside the scope of this example.

**REST API Tests (folder setup)** — a Docker step starts the stub database: a dedicated PostgreSQL container, with database `online_store` created automatically. A database step then creates the `online_store` schema. Because this stub database belongs to the tests alone, the APIs run against a known, isolated dependency rather than a shared one. It sits at the top of the API tests so the stub database is provisioned in one place.

**User Preferences API (folder setup)** — one database step creates the `user_preferences` table in the stub database. It lives here, and only here, because no other API touches this table.

**Common Test Setups / Create Orders Table (test case)** — a standalone test case, in its own folder *outside* REST API Tests, with a database step that creates the `orders` table in the stub database. It's written to be repeatable — it drops the table first, then creates it — so it's safe to run more than once.

**Create Order API and Update Order Status API (setup references)** — both folders reference "Create Orders Table" instead of duplicating its DDL. Because the referenced test case lives outside REST API Tests, this also shows that setup references aren't limited to a folder's own subtree.

## Running the Tests

A folder's **Run Pattern** controls what a run includes — which setups run, and whether test cases run with them. ATB offers three:

* **Setups to Here** — runs every setup defined or referenced from the workspace level down to the folder, and no test cases. It prepares the environment without running any test.
* **Default** — runs the folder's own setups, then recurses through everything beneath it: each descendant sub-folder's setups and all test cases. It does not run ancestor setups.
* **All** — the same as Default, plus the ancestor setups first. A fully self-contained run.

One current limitation to note: running a single test case on its own does not run any folder setups — neither its own folder's nor its ancestors'. Preparing those setups is what **Setups to Here** is for.

### During API development

While building an API, the developer launches and debugs the implementation in their IDE and uses ATB test cases to verify each change. They run the API folder once with **Setups to Here** to stand up the stub database and create the tables — for Create Order API, that's the REST API Tests folder setup (Docker + schema) plus the folder's own "Create Orders Table" reference. It runs no test cases.

From there they iterate quickly. Running a single test case re-runs no setup at all. Running the API folder with **Default** runs the folder's own setups and all of its test cases, but not the ancestor setups (the Docker container and schema) — so the expensive container start-up isn't repeated between iterations. When a setup itself changes, the developer re-runs **Setups to Here** to apply it before continuing.

### In CI/CD

A pipeline runs a folder with **All**, so the run is self-contained: the ancestor setups run first, then the folder's own setups and every test case beneath it.

Pointing this at a single API folder (say, Create Order API) runs just that API's setups and tests — useful when each API has its own Git repository, so a change to one API doesn't trigger the whole suite. Pointing it at the REST API Tests folder runs everything, for a full pass across all APIs.

One note for the full run: setup deduplication isn't implemented yet, so "Create Orders Table" runs once for each folder that references it — twice across Create Order API and Update Order Status API. Because the setup drops and recreates the table, the repeat is harmless.

## Why This Matters

Each table's setup lives in exactly one place, owned by whichever folder(s) actually depend on it — not duplicated, and not buried inside individual test cases. Dependencies between folders are explicit through setup references rather than assumed. The stub database keeps every run isolated from shared environments, and the same structure that provisions it also lets each test case verify the resulting row in `user_preferences` or `orders` as a side effect of the API call. That combination of isolation and side-effect verification is the heart of what ATB is for.
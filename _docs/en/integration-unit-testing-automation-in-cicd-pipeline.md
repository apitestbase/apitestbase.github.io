---
title: Integration Unit Testing Automation in CI/CD Pipeline
permalink: /docs/en/integration-unit-testing-automation-in-cicd-pipeline
key: docs-integration-unit-testing-automation-in-cicd-pipeline
---

This page shows how to automate
[**Integration Unit Testing**](/docs/en/what-is-integration-unit-testing) (IUT)
of an API in a CI/CD pipeline. Integration unit testing exercises an API as a
black box, in isolation, and verifies both **the API's contract** (its responses)
and **the API's side effects** (database records written, messages sent to a
queue, files written, downstream APIs called). The API runs against stub
dependencies so every run starts from a known, fully controlled state.

Integration unit tests don't require a pipeline. During API development they're
run by hand &mdash; for instance a single test case, to confirm a refactor didn't break
a piece of functionality. Automating them in a CI/CD pipeline, to run the whole
set in one go, is what this page covers.

API Test Base (ATB) drives these tests through its own REST API &mdash; the same API
the ATB UI calls &mdash; so the test cases you author and debug locally run unchanged
in the pipeline. The API under test, ATB, and the stub dependencies all run
together on a single pipeline runner and talk to each other over `localhost`. The
script deploys and starts the API under test, starts ATB headless, and triggers
an IUT run over ATB's REST API, failing the pipeline when the run fails. This page
uses the [no-JRE build](/docs/en/quick-start) and finishes with a single,
portable shell script you can adapt to any CI tool.

## What the pipeline does

An integration unit test run on the pipeline runner has three responsibilities,
split between your script and ATB:

- **Deploy the API under test &mdash; your script's job.** Build (optional) and deploy
  (mandatory) the API into a runtime on the pipeline runner, then start it. ATB
  does not do this, by design: during API development the API is deployed and
  debugged from the developer's IDE, which has an embedded runtime (for example a
  Spring Boot app's embedded Tomcat, or a Mule runtime), so the developer can
  watch the logs in the console and step through the code with the debugger.
  There is nothing for ATB to deploy locally. A pipeline has no IDE, so the
  script reproduces that deployment.
- **Set up the stub dependencies &mdash; ATB's job.** The stub database, message
  broker, and so on that isolate the API are stood up by the workspace's setup
  steps (for example a Docker step that starts a throwaway container). The exact
  same setup logic developers run locally runs in the pipeline &mdash; nothing is
  duplicated. See
  [Structured Test Setup for Integration Unit Test Isolation](/docs/en/structured-test-setup-for-integration-unit-test-isolation)
  for how to organise these setups, and [Folder Run Patterns](/docs/en/folder-run-patterns)
  for how a run decides which setups to execute.
- **Run the tests and check the results &mdash; ATB's job, triggered by your script.**
  Your script calls ATB's REST API to run the tests; ATB drives the API and
  asserts both its responses and its side effects, then returns the result.

The ATB test run is **synchronous**: the REST API call that triggers it blocks
until the whole run has finished and returns the complete result, so there is no
status to poll.

> The API is deployed before the ATB run, and a single run then performs both the
> stub-dependency setups and the test cases. This works even though the stubs
> don't exist yet when the API starts: most APIs log a connection error and then
> reconnect automatically once the stubs are up, so the test cases that follow are
> unaffected. If your API can't reconnect on its own, add a step that restarts the
> API instance to the setup of the folder holding its test cases, so it connects
> to the freshly-created stubs before any test case runs &mdash; still one run, one
> report.

## Prerequisites

- `Java 21+` on the pipeline runner (required by the no-JRE build).
- `curl`, `jq`, `git`, and `unzip` available on the runner.
- Whatever your API needs to run on the runner &mdash; its own runtime, or a runtime
  the script installs.
- A container engine such as Docker on the runner, if your stub dependencies are
  containers.
- Your tests saved as a workspace and pushed to a git repository (see
  [Team Collaboration](/docs/en/team-collaboration)), with a dedicated ATB
  [environment](/docs/en/environments-management) defined in it whose endpoints
  point at the deployed API and the stub dependencies. This page assumes that
  environment is named `CICD`.

## Install ATB on the runner

Download `apitestbase-<version>-allos-nojre.zip` from the
[release page](https://github.com/apitestbase/apitestbase-release/releases) and
extract it into the directory you'll use as ATB's data directory, `ATB_DATA_DIR`
(see [Administration](/docs/en/administration)). The archive provides `start.sh`
(Linux/macOS) and `start.bat` (Windows) for launching ATB. Test workspaces live
under `$ATB_DATA_DIR/fileplace`, which a fresh extract doesn't include &mdash; the
script creates it when it adds the workspace (the next step).

ATB serves everything on a single **app port** (default `8090`): the REST API
under `/api/...` and the UI under `/ui`. The port can be overridden with the
`ATB_App_Port` environment variable when the default clashes with something
else on the runner (for instance the API under test).

## Add your test workspace

Clone your workspace repository into ATB's `fileplace` directory. The folder name
it lands under is the workspace's id, and the first path segment of the folder
and environment ids you pass when triggering a run:

```bash
git clone https://github.com/your-org/my-iut-workspace.git \
    "$ATB_DATA_DIR/fileplace/my-iut-workspace"
```

Keeping the workspace in git is what makes the tests version-controlled and
reproducible from run to run. For a private repository, supply credentials the
way your CI tool expects &mdash; for example a token in the clone URL or a deploy key &mdash;
and store any such token as a masked/secret variable rather than committing it.

## Deploy the API under test

This step depends on your API's technology and is your script's responsibility.
Build the API from source if the pipeline doesn't already have a built artifact,
deploy it into a runtime, start it, and wait until it is up before running the
tests. The example below builds and starts a Spring Boot API (whose embedded
server makes the built jar self-deploying); a Mule app, a Tomcat WAR, or any
other stack follows the same shape &mdash; build, deploy, start, wait.

```bash
# Optional: build from source
mvn -q -f api-under-test/pom.xml package -DskipTests

# Deploy and start the API (Spring Boot embedded server)
nohup java -jar api-under-test/target/*.jar >/dev/null 2>&1 & echo $! > api.pid

# Wait until the API is up
for i in $(seq 1 30); do
    curl -sf -o /dev/null "http://localhost:8081/actuator/health" && break
    sleep 2
done
```

The `CICD` ATB environment in your workspace should point its endpoints at this
deployed API (here `http://localhost:8081`) and at the stub dependencies ATB
stands up during the run.

## Start ATB and wait until it is ready

Start ATB with its `start.sh` script in the background, then poll the app port
until it answers (it starts answering only once ATB has fully started):

```bash
( cd "$ATB_DATA_DIR" && nohup ./start.sh >/dev/null 2>&1 & echo $! > atb.pid )

for i in $(seq 1 30); do
    curl -sf -o /dev/null "http://localhost:8090/ui" && break
    sleep 2
    [ "$i" -eq 30 ] && { echo "ATB did not start in time."; exit 1; }
done
```

## Run the integration unit tests

Trigger a run with a `POST` to the app port. The recommended approach for CI is
to run a **folder** with the `All` pattern, which makes the run self-contained:
the folder's ancestor setups run first (these stand up the stub dependencies),
then the folder's own setups and every test case beneath it. Pointing this at a
single API's folder runs just that API's setups and tests &mdash; handy when each API
has its own repository.

```bash
response=$(curl -sS -X POST -G "http://localhost:8090/api/folderruns" \
    --data-urlencode "folderId=my-iut-workspace/REST API Tests" \
    --data-urlencode "environmentId=my-iut-workspace/CICD" \
    --data-urlencode "pattern=All")
```

- `folderId` is `<workspace name>/<folder path>`.
- `environmentId` is `<workspace name>/<environment name>` (or `/1` for "No
  Environment").
- `pattern` is one of `Default`, `All`, or `SetupsToHere`. See
  [Folder Run Patterns](/docs/en/folder-run-patterns) for what each one runs.

The call returns only once the run has finished, with a JSON document describing
the whole run.

To run **every** folder in the workspace instead of one subtree, post to
`/api/workspaceruns` with `workspaceId` and `environmentId` (no pattern). The
response has the same shape.

## Fail the pipeline on failure

The top-level `result` field is `Passed`, `Failed`, or `Cancelled`. Map anything
other than `Passed` onto a non-zero exit code so the pipeline goes red:

```bash
result=$(echo "$response" | jq -r '.result')
[ "$result" = "Passed" ] && exit 0 || exit 1
```

For a console summary, the `testProcedureRuns` array lists each test case
(`type == "TR"`). A failed test case that ran is a **failure**; a failed test
case with no `duration` did not run properly and counts as an **error**:

```bash
total=$(echo "$response"    | jq '[.testProcedureRuns[] | select(.type=="TR")] | length')
failures=$(echo "$response" | jq '[.testProcedureRuns[] | select(.type=="TR" and .result=="Failed" and has("duration"))] | length')
errors=$(echo "$response"   | jq '[.testProcedureRuns[] | select(.type=="TR" and .result=="Failed" and (has("duration") | not))] | length')
echo "Test cases: $total, Failures: $failures, Errors: $errors"
```

## Download the test report

The run call returns a JSON response carrying the run id in its `id` field. Use
it to download the report as a zip, then unzip it and publish the folder as a
pipeline artifact. A folder run is downloaded from
`/folderruns/htmlreport/download`; a workspace run from
`/workspaceruns/htmlreport/download` (with `workspaceRunId`):

```bash
folderRunId=$(echo "$response" | jq -r '.id')
mkdir -p test-reports
curl -sSOJ --output-dir test-reports -G \
    "http://localhost:8090/api/folderruns/htmlreport/download" \
    --data-urlencode "folderRunId=$folderRunId"
( cd test-reports && unzip -o ./*.zip >/dev/null && rm -f ./*.zip )
```

Because the setups and test cases ran in a single call, this one report covers
the whole run: the zip's `index.html` lists every setup and test case that ran,
each linking to its own page with the step-by-step detail &mdash; request and response
for each step, any error, the duration, and a Passed/Failed result.

## Secrets

A secret defined on an ATB environment keeps its value out of the workspace repo:
the environment YAML stores only a reference, and the encrypted value lives in a
local `secrets.properties` file that is never committed. See
[Environments Management](/docs/en/environments-management) for how secrets are
defined and stored.

Developers keep their own private environment, typically `Local`, and add
`Local.yaml` to `.gitignore` so it never reaches the repo. The shared `CICD`
environment (`environments/CICD.yaml`) is the one committed and pushed, and the
one the pipeline runs against.

A fresh pipeline runner has no `secrets.properties`, so the committed `CICD`
environment's secret references can't be resolved there. Supply each value at run
time through an environment variable named `ATB_ENV_PROP_<property>`, set from a
masked CI/CD variable. ATB reads variables with the `ATB_ENV_PROP_` prefix from
its process environment and overrides the matching environment property for the
run. The name is matched fuzzily, so case and separators don't have to line up
&mdash; `ATB_ENV_PROP_db_password` resolves to the environment property `db.password`:

```bash
export ATB_ENV_PROP_db_password="$DB_PASSWORD"   # $DB_PASSWORD from a masked pipeline variable
```

Set this before ATB starts. The real secret stays in the pipeline's secret store
and never touches git.

## Full example

The script below combines the steps into one portable file. Point the variables
at your own API, workspace, folder, and repository, and use its exit code as the
pipeline's pass/fail gate. The deploy block is stack-specific &mdash; adapt it to your
API.

```bash
#!/usr/bin/env bash
# Automate Integration Unit Testing of an API in a CI/CD pipeline.
set -euo pipefail

# ---- Configuration -------------------------------------------------------
ATB_VERSION="0.39.2"
ATB_DATA_DIR="$(pwd)/atb-${ATB_VERSION}"        # release extracted here; also holds fileplace/
export ATB_DATA_DIR
WORKSPACE_NAME="my-iut-workspace"                # folder name under fileplace/
WORKSPACE_REPO="https://github.com/your-org/my-iut-workspace.git"
FOLDER_ID="${WORKSPACE_NAME}/REST API Tests"     # folder to run
ENVIRONMENT_ID="${WORKSPACE_NAME}/CICD"          # environment defined in the workspace
APP_PORT=8090                                    # ATB REST API (/api/...) and readiness (/ui)
API_HEALTH_URL="http://localhost:8081/actuator/health"   # API under test
REPORTS_DIR="$(pwd)/test-reports"
API_PID="$(pwd)/api.pid"
ATB_PID="$(pwd)/atb.pid"

# ---- 1. Deploy the API under test (adapt to your stack) -----------------
echo "Deploying the API under test..."
mvn -q -f api-under-test/pom.xml package -DskipTests
nohup java -jar api-under-test/target/*.jar >/dev/null 2>&1 & echo $! > "$API_PID"
trap 'kill "$(cat "$API_PID")" "$(cat "$ATB_PID" 2>/dev/null)" 2>/dev/null || true' EXIT
for i in $(seq 1 30); do
    curl -sf -o /dev/null "$API_HEALTH_URL" && break
    sleep 2
    [ "$i" -eq 30 ] && { echo "API did not start in time."; exit 1; }
done

# ---- 2. Download and extract the ATB no-JRE build -----------------------
if [ ! -d "$ATB_DATA_DIR" ]; then
    echo "Downloading ATB ${ATB_VERSION}..."
    mkdir -p "$ATB_DATA_DIR"
    curl -sSL -o "$ATB_DATA_DIR/atb.zip" \
        "https://github.com/apitestbase/apitestbase-release/releases/download/${ATB_VERSION}/apitestbase-${ATB_VERSION}-allos-nojre.zip"
    ( cd "$ATB_DATA_DIR" && unzip -q atb.zip && rm atb.zip )
fi

# ---- 3. Place the test workspace in fileplace ---------------------------
rm -rf "$ATB_DATA_DIR/fileplace/$WORKSPACE_NAME"
git clone "$WORKSPACE_REPO" "$ATB_DATA_DIR/fileplace/$WORKSPACE_NAME"

# ---- 4. Start ATB in the background -------------------------------------
export ATB_App_Port=$APP_PORT
# export ATB_ENV_PROP_db_password="$DB_PASSWORD"   # inject a secret if your tests need one
( cd "$ATB_DATA_DIR" && nohup ./start.sh >/dev/null 2>&1 & echo $! > "$ATB_PID" )
for i in $(seq 1 30); do
    if curl -sf -o /dev/null "http://localhost:${APP_PORT}/ui"; then
        echo "ATB is up."
        break
    fi
    sleep 2
    [ "$i" -eq 30 ] && { echo "ATB did not start in time."; exit 1; }
done

# ---- 5. Run the integration unit tests (synchronous) --------------------
echo "Running folder '${FOLDER_ID}'..."
response=$(curl -sS -X POST -G "http://localhost:${APP_PORT}/api/folderruns" \
    --data-urlencode "folderId=${FOLDER_ID}" \
    --data-urlencode "environmentId=${ENVIRONMENT_ID}" \
    --data-urlencode "pattern=All")

# ---- 6. Download the report zip -----------------------------------------
folderRunId=$(echo "$response" | jq -r '.id')
mkdir -p "$REPORTS_DIR"
curl -sSOJ --output-dir "$REPORTS_DIR" -G \
    "http://localhost:${APP_PORT}/api/folderruns/htmlreport/download" \
    --data-urlencode "folderRunId=${folderRunId}"
( cd "$REPORTS_DIR" && unzip -o ./*.zip >/dev/null && rm -f ./*.zip )

# ---- 7. Summary and exit code -------------------------------------------
total=$(echo "$response"    | jq '[.testProcedureRuns[] | select(.type=="TR")] | length')
failures=$(echo "$response" | jq '[.testProcedureRuns[] | select(.type=="TR" and .result=="Failed" and has("duration"))] | length')
errors=$(echo "$response"   | jq '[.testProcedureRuns[] | select(.type=="TR" and .result=="Failed" and (has("duration") | not))] | length')
result=$(echo "$response"   | jq -r '.result')

echo "Test cases: ${total}, Failures: ${failures}, Errors: ${errors}"
echo "Result: ${result}"
[ "$result" = "Passed" ] && exit 0 || exit 1
```

## Adapting to your CI tool

Any CI runner that can execute a shell script will work &mdash; nothing above is tied to
a particular platform. To wire it in:

- Run the script as a pipeline step and let its **exit code** gate the pipeline.
- Store secrets (a repository token, the database password, and so on) as
  **masked variables** and surface each one to the script &mdash; secrets the tests
  consume go in as `ATB_ENV_PROP_<property>` variables.
- Publish the `test-reports` folder as a **pipeline artifact** so the HTML report
  is available from the pipeline's run summary.

On an ephemeral runner or container you can skip explicit teardown &mdash; the runner
is discarded when the job ends. On a long-lived self-hosted runner, stop the API
and ATB processes at the end of the job (the example does this on exit via a
`trap`).
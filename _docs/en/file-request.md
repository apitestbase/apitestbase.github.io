---
title: File Request
permalink: /docs/en/file-request
key: docs-file-request
---
File request is used to do operations on local files, i.e. files on the file system of the machine where API Test Base runs.

A File request can be used for ad hoc file operations, or in a test case to work with files around the API under test — for example, verifying a file that the API writes as its side effect, or touching a file to make a runtime redeploy the API (see [Integration Unit Testing Automation in CI/CD Pipeline](/docs/en/integration-unit-testing-automation-in-cicd-pipeline)).

Actions available: **Read**, **Write**, **Delete**, **Check Existence**, **Touch**, **Tail Until**.

## Read Action
The file content is read as text. When the File request is used as a test step, you can create [assertions](/docs/en/assertions) against the read content.

## Write Action
The file content is entered as text, in which you can use [property](/docs/en/properties) references.

## Tail Until Action
The Tail Until action keeps tailing the file until a line matching the pattern appears. The `Pattern` dropdown offers two kinds of matching: `Line Contains` matches a line containing the given substring, and `Line Matches Regex` matches a line against the given regular expression. If no matching line appears before the timeout you specify, the request fails with an error.

Tail Until saves you from guessing a fixed wait time when the outcome you are waiting for appears asynchronously in a file. Typical uses are verifying that the API under test outputs expected log lines, and confirming that a runtime has finished redeploying an API — for example, tailing the Mule runtime's log until it reports that the redeployment, triggered by touching a file in the Mule app, is finished.
---
title: FTP Request
redirect_from: /docs/en/ftp-test-step
permalink: /docs/en/ftp-request
key: docs-ftp-request
---
FTP request is used to do FTP operations like uploading a file.

An FTP request can be used for ad hoc file upload, replacing a separate FTP client tool, or in a test case to drop a file that triggers the API under test, whose side effects can then be verified in the same test case — see [Test APIs triggered by queue messages and file drops](/#test-apis-triggered-by-queue-messages-and-file-drops).

The FTP client library is bundled with API Test Base, so no setup is needed.

Actions available: **Put**.

## Put Action
You can provide the file content in two ways: by entering text, or by uploading a local file. When entering text, you can use [property](/docs/en/properties) references in the text. When uploading a local file, the file is not parsed but passed to the FTP server byte for byte, so it can be of any format, whether text or binary.

The action uses passive mode to transfer the file to the FTP server.

### Remote File Path
An FTP server exposes a specific directory of the server machine as its root directory, and all uploaded files go under that directory.

To create the remote file directly under the root directory, enter `filename` or `/filename`. To create it under a subdirectory, enter `path/to/the/filename` or `/path/to/the/filename`. Ensure the directory exists on the FTP server, otherwise the FTP server returns an error.
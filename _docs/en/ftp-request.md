---
title: FTP Request
redirect_from: /docs/en/ftp-test-step
permalink: /docs/en/ftp-request
key: docs-ftp-request
---
FTP request is used to do FTP operations like uploading a file.

The FTP client library is bundled with API Test Base, so no setup is needed.

Actions available: **Put**.

## Put Action
You can provide the file content in two ways: by entering text, or by uploading a local file. When entering text, you can use [property](/docs/en/properties) references in the text. When uploading a local file, the file is not parsed but passed to the FTP server byte for byte, so it can be of any format, whether text or binary.

The action uses passive mode to transfer the file to the FTP server.

### Target File Path
To create a remote file under the root path, enter `filename`.

To create a remote file under a specified path, enter `/path/to/the/filename`. Ensure the directory exists on the FTP server, otherwise the FTP server returns an error.
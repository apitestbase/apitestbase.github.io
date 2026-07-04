---
title: SFTP Request
redirect_from: /docs/en/sftp-test-step
permalink: /docs/en/sftp-request
key: docs-sftp-request
---
SFTP request is used to do SFTP operations like uploading a file.

An SFTP request can be used for ad hoc file upload, replacing a separate SFTP client tool, or in a test case to drop a file that triggers the API under test, whose side effects can then be verified in the same test case — see [Test APIs triggered by queue messages and file drops](/#test-apis-triggered-by-queue-messages-and-file-drops).

The SFTP client library is bundled with API Test Base, so no setup is needed.

Actions available: **Put**.

## Put Action
You can provide the file content in two ways: by entering text, or by uploading a local file. When entering text, you can use [property](/docs/en/properties) references in the text. When uploading a local file, the file is not parsed but passed to the SFTP server byte for byte, so it can be of any format, whether text or binary.

### Remote File Path
A relative path like `path/to/the/filename` creates the remote file under the login user's home directory on the SFTP server.

An absolute path like `/path/to/the/filename` refers to a location on the SFTP server machine's file system. Whether it is writable depends on the server's configuration: a plain SSH/SFTP server (like macOS with Remote Login enabled) allows writing to any location the login user has write permission on, while a server that confines (chroots) the session to a directory — the atmoz/sftp Docker image, for example — rejects absolute paths outside the permitted directories.

Ensure the target directory exists on the SFTP server, otherwise the request fails with an error.
---
title: FTP Request
redirect_from: /docs/en/ftp-test-step
permalink: /docs/en/ftp-request
key: docs-ftp-request
---
FTP request is used to do FTP operations like uploading a file.

Actions available: **Put**.

## Put Action
You can upload file from text or from file. The action uses passive mode to transfer file to the FTP server.

### Target File Path
To create a remote file under the root path, enter 'filename'.
To create a remote file under specified path, enter '/path/to/the/filename'. Ensure the directory exists on the FTP server, otherwise you could see error returned from the FTP server.
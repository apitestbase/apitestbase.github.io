---
title: FTP Test Step
permalink: /docs/en/ftp-test-step
key: docs-ftp-test-step
---
FTP test step is normally used in combination with other test steps to create automated test cases. However, you can also use it to manually do FTP operations like uploading a file. (Actually, most types of test steps in Iron Test can be used to individually/manually invoke target API)

Actions available in the FTP test step: **Put**.

## Put Action
You can upload file from text or from file. The action uses passive mode to transfer file to the FTP server.

### Target File Path
To create a file under the root path, enter 'filename'.
To create a file under specified path, enter '/path/to/the/filename'. Ensure the directory exists on the FTP server, otherwise you could see error returned from the FTP server.
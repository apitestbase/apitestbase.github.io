---
title: Administration
redirect_from: /docs/en/maintenance
permalink: /docs/en/administration
key: docs-administration
---
## ATB Data Directory
API Test Base stores data in a folder called `<ATB_DATA_DIR>`. Its default value depends on the ATB build:

{% tabs data-folder %}

{% tab data-folder macOS %}

```
~/Library/Application Support/API Test Base
```

{% endtab %}

{% tab data-folder Windows %}

```
%APPDATA%\ApiTestBase
```

{% endtab %}

{% tab data-folder Docker %}

```
/atb/data
```

This is a path inside the Docker container. It is used by the ATB application (like in the endpoint of a database request for invoking the ATB bundled sample H2 database). The `-v` parameter of the `docker run` command (refer to [Quick Start](/docs/en/quick-start)) maps it to a folder on the host machine, so the data can be checked and edited from the host - for example, setting values in the `config.properties` file, or editing a request YAML file under the `fileplace` folder.

{% endtab %}

{% tab data-folder Java %}

Depends on the OS where API Test Base runs:

```
// Windows
%APPDATA%\ApiTestBase

// macOS
~/Library/Application Support/API Test Base

// Linux
~/.local/share/API Test Base
```

{% endtab %}

{% endtabs %}

When using the ATB desktop application, you can open the data folder directly via menu `Help > Open Data Folder`.

The first time you launch the application, the following folders are created automatically under the `<ATB_DATA_DIR>` folder.

* `database` - where a sample H2 database named `sample` is located. The sample database is for you to play with ATB basic features such as REST API testing or database testing. An `Article` table is in it.
* `fileplace` - where all your local workspaces are stored. Each folder under the `fileplace` folder is a workspace, which has the requests, test cases, environments, etc. you create for your testing work (refer to [Team Collaboration](/docs/en/team-collaboration)). Workspace files can also be edited directly - a running ATB automatically picks up the saved changes and reflects them on the ATB UI. The `secrets.properties` file holding [secret](/docs/en/environments-management) values is also located directly under this folder, but only under the default `<ATB_DATA_DIR>` (see [Changing \<ATB_DATA_DIR\>](#changing-atb_data_dir) below).
* `logs` - where ATB application runtime logs are located.
* `lib` - where you put external libraries when needed (refer to [Interact with Other Systems](/docs/en/interact-with-other-systems)).
* `electron` - where the Electron app data and logs are located. Only applicable to the ATB desktop application.

In addition, a `config.properties` file is copied into `<ATB_DATA_DIR>` at launch if not already there (see [Changing Configurations](#changing-configurations) below).

## Changing Configurations
To change configurations like port numbers, edit the `<ATB_DATA_DIR>/config.properties` file (uncommenting the relevant options and setting their values), and restart ATB. For example, to change the port ATB serves its UI and REST APIs on (default `8090`):

```
atb.app.port=9090
```

Another example is the truststore password used for SSL connections - refer to [Configure API Test Base to be an SSL Client](/docs/en/configure-api-test-base-to-be-an-ssl-client).

## Changing \<ATB_DATA_DIR\>
To change `<ATB_DATA_DIR>` to a different location, set an environment variable `ATB_DATA_DIR=/path/to/the/new/location` on your operating system, and restart ATB. This works for every ATB build. For the Docker build, set the environment variable when running the container (for example, via the `-e` parameter of the `docker run` command), and notice that the value is a path inside the container rather than on the host machine.

Note that the `secrets.properties` file stays under the `fileplace` folder of the default `<ATB_DATA_DIR>` even when `ATB_DATA_DIR` is changed (refer to [Environments Management](/docs/en/environments-management)).

`macOS users`: to set environment variables for applications (launched from Spotlight, Dock, etc.), create a plist file under ~/Library/LaunchAgents folder, and restart the machine. For example: io.apitestbase.plist

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
    <dict>
        <key>Label</key>
        <string>io.apitestbase.env</string>
        <key>ProgramArguments</key>
        <array>
            <string>/bin/launchctl</string>
            <string>setenv</string>
            <string>ATB_DATA_DIR</string>
            <string>/Users/your-username/atb-data</string>
        </array>
        <key>RunAtLoad</key>
        <true/>
    </dict>
</plist>
```
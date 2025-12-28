---
title: Administration
redirect_from: /docs/en/maintenance
permalink: /docs/en/administration
key: docs-administration
---
API Test Base application stores data in a folder called `<ATB_DATA_DIR>`. Following is the default value:

{% tabs data-folder %}

{% tab data-folder Windows %}

```
%USERPROFILE%\AppData\Roaming\ApiTestBase
```

{% endtab %}

{% tab data-folder macOS %}

```
~/Library/Application Support/API Test Base
```

{% endtab %}

{% tab data-folder Docker %}

There are two `<ATB_DATA_DIR>`s.

One is inside the Docker container, with default value `/atb/data`. It is used by ATB application (like in the endpoint of a database request for invoking the ATB bundled sample H2 database).

The other one is on the host and mapped to `/atb/data` in the `-v` parameter when running the `docker run` command (refer to [Quick Start](/docs/en/quick-start)). This one is for human reference.

{% endtab %}

{% tab data-folder Java %}

The directory where the downloaded apitestbase-&lt;version&gt;-allos-nojre.zip was extracted to.

{% endtab %}

{% endtabs %}

When using ATB desktop application, you can open the data folder directly via menu `Help > Open Data Folder`.

The first time you launch the application, below new folders are created automatically under the `<ATB_DATA_DIR>` folder.

```
database - where a sample H2 database is located.
    Sample database is for you to play with ATB basic features such as REST API testing or database testing. An Article table is in it.

fileplace - where all your local workspaces are stored
    Each folder under the fileplace folder is a workspace.
    A workspace has requests, test cases, environments, etc. you create for your testing work.

logs - where ATB application runtime logs are located.
   
lib - where you put external libraries when needed.

electron - where the Electron app data and logs are located. Not applicable to ATB Docker container.
```

#### Changing Configurations
To change configurations like port numbers, modify the `<ATB_DATA_DIR>/config.properties` file content, and restart ATB.

#### Changing \<ATB_DATA_DIR\>
To change `<ATB_DATA_DIR>` to a different location, set an environment variable `ATB_DATA_DIR=/path/to/the/new/location` on your operating system, and restart ATB.

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
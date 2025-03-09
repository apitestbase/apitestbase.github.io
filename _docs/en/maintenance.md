---
title: Maintenance
permalink: /docs/en/maintenance
key: docs-maintenance
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

The `/data/folder/on/host` you provided in the `docker run` command.

{% endtab %}

{% endtabs %}

The first time you launch the application, below new folders are created automatically under the `<ATB_DATA_DIR>` folder.

```
database - where system database and a sample database are located. Both are H2 databases. 
    System database is used to store all test cases, environments, endpoints, etc. you create using ATB.
    Sample database is for you to play with ATB basic features such as REST API testing or database testing. An Article table is in it.
   
logs - where ATB application runtime logs are located.
   
lib - where you put external libraries when needed.

electron - where the Electron app data and logs are located. Not applicable to ATB Docker container.
```

It is recommended that you back up `<ATB_DATA_DIR>/database` folder regularly. Remember to exit the application before backing up.

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
            <string>/Users/zhengwang/atb-data</string>
        </array>
        <key>RunAtLoad</key>
        <true/>
    </dict>
</plist>
```
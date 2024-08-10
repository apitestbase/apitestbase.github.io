---
title: Quick Start
permalink: /docs/en/quick-start
key: docs-quick-start
---
## Install and Launch

{% tabs install %}

{% tab install Windows %}

Download installer `apitestbase-{{ site.atb_release_version }}.exe` from the [latest release page](https://github.com/apitestbase/apitestbase-release/releases/tag/{{ site.atb_release_version }}){:target="_blank"}.

Double click the installer and follow through the normal Windows application installation process. You will likely get a warning dialog from Windows Defender SmartScreen, because the app hasn't been digitally signed by a Microsoft trusted Certificate Authority (which is not free). Click 'More info' on the dialog, and click 'Run anyway' to proceed installing API Test Base.

Once the installation finishes, you can launch API Test Base from Start Menu or Desktop shortcut.

First time launching, a Windows Defender Firewall alert will pop up. Check the `Private networks ...` option, uncheck the `Public networks ...` option, and click the `Allow access` button.

Once the application is launched, a system tray icon shows. Click the icon to bring up the menu, like below.

![System Tray Menu](../../screenshots/install-and-launch/system-tray-menu.png)

Select `Open ATB` to open API Test Base UI in your default browser.

{% endtab %}

{% tab install Mac OS %}

Download `apitestbase-{{ site.atb_release_version }}.dmg` from the [latest release page](https://github.com/apitestbase/apitestbase-release/releases/tag/{{ site.atb_release_version }}){:target="_blank"}.

Double click the dmg and copy `API Test Base.app` to your /Applications folder.

Open the `/Applications/API Test Base.app`, and you'll see a warning dialog stating '"API Test Base.app" can't be opened because Apple cannot check it for malicious software'. This is because the app hasn't been digitally signed using Apple certificate (which is not free). Below steps enable you to launch API Test Base.

Click OK on the dialog, open System Settings, go to Privacy & Security, and you'll see a warning message 'API Test Base.app was blocked from use because it is not from an identified developer'.

Click the Open Anyway button. If prompted for login password (for approving the app opening), enter the password, and you'll see the first dialog again, but with an Open button.

Click the Open button to launch API Test Base.

After successful launch, you'll no longer see the dialogs when launching again.

Once the application is launched, a system tray icon shows. Click the icon to bring up the menu, like below.

![System Tray Menu](../../screenshots/install-and-launch/system-tray-menu.png)

Select `Open ATB` to open API Test Base UI in your default browser.

{% endtab %}

{% tab install Docker %}

Windows or Mac OS host

```
docker run -d -t -v /data/folder/on/host:/atb/data --name apitestbase-{{ site.atb_release_version }} -p 8090:8090 apitestbase/apitestbase:{{ site.atb_release_version }}
```

Linux host

```
docker run -d -t -v /data/folder/on/host:/atb/data --add-host=host.docker.internal:host-gateway --name apitestbase-{{ site.atb_release_version }} -p 8090:8090 apitestbase/apitestbase:{{ site.atb_release_version }}
```

Notice: Replace `/data/folder/on/host` with your own value. For example (Windows host):

```
docker run -d -t -v C:\Users\zheng\AppData\Roaming\ApiTestBaseDocker:/atb/data --name apitestbase-{{ site.atb_release_version }} -p 8090:8090 apitestbase/apitestbase:{{ site.atb_release_version }}
```

This is so that your data can be retained on the host machine when the container is deleted.

Once the container is running, open `http://localhost:8090/ui` in a Chrome browser on the host machine.

{% endtab %}

{% endtabs %}

## Ad Hoc Invocation
Suppose you want to invoke a REST API.

Right click anywhere in the left side pane, select `New Request` > HTTP, give it a name and press Enter. The request is created:

![New HTTP Request](../../screenshots/basic-use/new-http-request.png)

In the request edit view, under the Invocation tab, select Method `GET`, enter a URL like `http://localhost:8090/api/articles` (an API Test Base bundled API) and click the `Invoke` button.

![Ad Hoc HTTP Invocation](../../screenshots/basic-use/ad-hoc-http-invocation.png)

Docker users: to use ATB docker container to call your API on the host machine, use `host.docker.internal` as hostname in the request URL.
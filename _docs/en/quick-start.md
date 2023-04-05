---
title: Quick Start
permalink: /docs/en/quick-start
key: docs-quick-start
---
## Install and Launch
Download the latest API Test Base installer from the [release page](https://github.com/apitestbase/apitestbase-release/releases/tag/{{ site.atb_release_version }}).

{% tabs install %}

{% tab install Windows %}

Download the installer `apitestbase-{{ site.atb_release_version }}.exe`, double click it and follow through the normal Windows application installation process.

Once the installation finishes, you can launch API Test Base from Start Menu or Desktop shortcut.

First time launching, a Windows Defender Firewall alert will pop up. Uncheck the `Public networks ...` option, and click the `Allow access` button.

{% endtab %}

{% tab install Mac OS %}
Coming soon ...
{% endtab %}

{% endtabs %}

Once the application is launched, a system tray icon shows. Click the icon to bring up the menu, like below.

![System Tray Menu](../../screenshots/install-and-launch/system-tray-menu.png)

Select `Open ATB` to open API Test Base UI in your default browser.

## Ad Hoc Invocation
If you only want to invoke an API and see its response, just need to create a test step in a test case.

Right click on a folder in the tree, select Create Test Case and give it a name.

![New Ad Hoc Test Case](../../screenshots/basic-use/new-ad-hoc-test-case.png)

Suppose you want to invoke a REST API. On the right pane of the screen, under the Test Steps tab, click the `Create` dropdown button and select `HTTP Step`. The test step edit view opens.

Under the Basic Info tab, enter the test step name. Under the Invocation tab, select Method like `GET`, enter a URL like `http://localhost:8090/api/articles` (an API Test Base bundled API) and click the `Invoke` button.

![Ad Hoc HTTP Invocation](../../screenshots/basic-use/ad-hoc-http-invocation.png)
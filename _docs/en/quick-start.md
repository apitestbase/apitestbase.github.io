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

{% endtab %}

{% tab install Mac OS %}

Download `apitestbase-{{ site.atb_release_version }}.dmg` from the [latest release page](https://github.com/apitestbase/apitestbase-release/releases/tag/{{ site.atb_release_version }}){:target="_blank"}.

Double click the dmg and copy `API Test Base.app` to your /Applications folder.

Open the `/Applications/API Test Base.app`, and you'll see a warning dialog stating '"API Test Base.app" can't be opened because Apple cannot check it for malicious software'. This is because the app hasn't been digitally signed using Apple certificate (which is not free). Below steps enable you to launch API Test Base.

Click OK on the dialog, open System Settings, go to Privacy & Security, and you'll see a warning message 'API Test Base.app was blocked from use because it is not from an identified developer'.

Click the Open Anyway button. If prompted for login password (for approving the app opening), enter the password, and you'll see the first dialog again, but with an Open button.

Click the Open button to launch API Test Base.

After successful launch, you'll no longer see the dialogs when launching again.

{% endtab %}

{% endtabs %}

Once the application is launched, a system tray icon shows. Click the icon to bring up the menu, like below.

![System Tray Menu](../../screenshots/install-and-launch/system-tray-menu.png)

Select `Open ATB` to open API Test Base UI in your default browser.

## Ad Hoc Invocation
If you only want to invoke an API and see its response, just need to create a test step in a test case.

Right click anywhere in the left side pane, select Create Test Case, give it a name and press Enter. The test case is created, like the following.

![New Ad Hoc Test Case](../../screenshots/basic-use/new-ad-hoc-test-case.png)

Suppose you want to invoke a REST API. On the right pane of the screen, under the Test Steps tab, click the `+ Test Step` dropdown button and select `HTTP`. Give the test step a name, and press Enter, like the following.

![New Ad Hoc Test Step](../../screenshots/basic-use/new-ad-hoc-test-step.png)

Click the test step name to open the test step edit view.

Under the Invocation tab, select Method like `GET`, enter a URL like `http://localhost:8090/api/articles` (an API Test Base bundled API) and click the `Invoke` button.

![Ad Hoc HTTP Invocation](../../screenshots/basic-use/ad-hoc-http-invocation.png)

---
title: Upgrade from Old Version
permalink: /docs/en/upgrade-from-old-version
key: docs-upgrade-from-old-version
---
## Upgrade

Before upgrade, make sure you `exit API Test Base app` if it is running.

Download the latest API Test Base installer from the [release page](https://github.com/apitestbase/apitestbase-release/releases/tag/{{ site.atb_release_version }}){:target="_blank"}.

{% tabs upgrade %}

{% tab upgrade Windows %}

Running a new version of the installer will automatically uninstall the old version and install the new version.

Your test data, settings, etc. will stay untouched, under the `%USERPROFILE%\AppData\Roaming\ApiTestBase` directory.

{% endtab %}

{% tab upgrade Mac OS %}

Open a new version of the dmg file, and you'll be able to copy the new version of `API Test Base.app` to your /Applications folder, `replacing` the old one.

Your test data, settings, etc. will stay untouched, under the `~/Library/Application Support/API Test Base` directory.

{% endtab %}

{% endtabs %}

After the upgrade, clear your browser cache files (clearing last one hour is enough). Launch API Test Base, and refresh UI page if it is open.
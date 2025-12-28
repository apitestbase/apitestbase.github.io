---
title: Upgrade from Old Version
permalink: /docs/en/upgrade-from-old-version
key: docs-upgrade-from-old-version
---
## Upgrade

{% tabs upgrade %}

{% tab upgrade Windows %}

`Exit API Test Base app` if it is running.

Download the latest API Test Base version from the [release page](https://github.com/apitestbase/apitestbase-release/releases/tag/{{ site.atb_release_version }}){:target="_blank"}.

For the installer (setup exe), running a new version installer will automatically uninstall the old ATB version and install the new version. It is typical for you to install the new version into the existing ATB installation directory.

For the portable, just abandon the old version and start using the new version.

{% endtab %}

{% tab upgrade macOS %}

`Exit API Test Base app` if it is running.

Download the latest API Test Base version from the [release page](https://github.com/apitestbase/apitestbase-release/releases/tag/{{ site.atb_release_version }}){:target="_blank"}.

Open the new dmg file, and you'll be able to drag the new version of `API Test Base.app` to your `/Applications` folder, `replacing` the old one.

{% endtab %}

{% tab upgrade Docker %}

`Stop or remove the existing API Test Base container` (to avoid port number conflict with the new container).

Run the new version following the instructions on [Quick Start](/docs/en/quick-start) page.

{% endtab %}

{% tab upgrade Java %}

When the build is used for API test automation in CI/CD pipeline, there is no need to upgrade, since the build is discarded after the pipeline run. You can just change the ATB version number in your pipeline definition file to use the latest ATB version.

If you use the build for other purposes like trying out ATB, the steps for upgrade are as follows.

`Stop the existing API Test Base instance` (to avoid port number conflict with the new instance).

Download `apitestbase-{{ site.atb_release_version }}-allos-nojre.zip` from the [release page](https://github.com/apitestbase/apitestbase-release/releases/tag/{{ site.atb_release_version }}){:target="_blank"}.

Extract the downloaded zip, and copy following folders and file from the old `<ATB_DATA_DIR>` to the new `<ATB_DATA_DIR>`:
```
database
fileplace
lib
logs
config.properties     # override target config.properties
``` 

Run the new version following the instructions on [Quick Start](/docs/en/quick-start) page.

{% endtab %}

{% endtabs %}

During ATB upgrade, your test data, settings, etc. will stay untouched, under the `<ATB_DATA_DIR>` folder (refer to [Administration](/docs/en/administration) for the location).
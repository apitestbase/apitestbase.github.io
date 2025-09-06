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

{% endtabs %}

Your test data, settings, etc. will stay untouched, under the `<ATB_DATA_DIR>` folder (refer to [Administration](/docs/en/administration) for the location).
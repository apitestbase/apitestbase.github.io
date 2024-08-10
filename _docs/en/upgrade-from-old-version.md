---
title: Upgrade from Old Version
permalink: /docs/en/upgrade-from-old-version
key: docs-upgrade-from-old-version
---
## Upgrade

{% tabs upgrade %}

{% tab upgrade Windows %}

`Exit API Test Base app` if it is running.

Download the latest API Test Base installer from the [release page](https://github.com/apitestbase/apitestbase-release/releases/tag/{{ site.atb_release_version }}){:target="_blank"}.

Running a new version installer will automatically uninstall the old ATB version and install the new version. It is typical for you to install the new version into the existing ATB installation directory.

When running the new version installer, you'll see the same security warning as before. Refer to [Quick Start](/docs/en/quick-start) for how to resolve it.

{% endtab %}

{% tab upgrade Mac OS %}

`Exit API Test Base app` if it is running.

Download the latest API Test Base dmg from the [release page](https://github.com/apitestbase/apitestbase-release/releases/tag/{{ site.atb_release_version }}){:target="_blank"}.

Open the new dmg file, and you'll be able to copy the new version of `API Test Base.app` to your /Applications folder, `replacing` the old one.

When launching the new version, you'll see the same security warning as before. Refer to [Quick Start](/docs/en/quick-start) for how to resolve it.

{% endtab %}

{% tab upgrade Docker %}

`Remove the existing API Test Base container`.

Run the new version following the instructions on [Quick Start](/docs/en/quick-start) page.

{% endtab %}

{% endtabs %}

Your test data, settings, etc. will stay untouched, under the `<APITestBase_Data>` folder (refer to [Maintenance](/docs/en/maintenance) for the location).
---
title: Quick Start
permalink: /docs/en/quick-start
key: docs-quick-start
---
## Install and Launch

{% tabs install %}

{% tab install Windows %}

Download API Test Base `{{ site.atb_release_version }}` from the [release page](https://github.com/apitestbase/apitestbase-release/releases/tag/{{ site.atb_release_version }}){:target="_blank"}.

When running the setup or portable binary, you will likely get a warning dialog from Microsoft Defender SmartScreen, because the app hasn't been digitally signed by a Microsoft trusted Certificate Authority (which is expensive!). Click `More info` on the dialog, and click `Run anyway` to proceed.

First time launching, a Windows Defender Firewall alert will pop up. Check the `Private networks ...` option, uncheck the `Public networks ...` option, and click the `Allow access` button.

{% endtab %}

{% tab install macOS %}

Download API Test Base `{{ site.atb_release_version }}` from the [release page](https://github.com/apitestbase/apitestbase-release/releases/tag/{{ site.atb_release_version }}){:target="_blank"}.

Double click the dmg and drag `API Test Base.app` to your `/Applications` folder.

{% endtab %}

{% tab install Docker %}

If you are using Docker Engine (Linux only) or Docker Desktop version 4.34 or later, the recommended way to launch API Test Base is with [host networking](https://docs.docker.com/engine/network/drivers/host/){:target="_blank"}.

```
docker run -d -t -v /data/folder/on/host:/atb/data --name apitestbase-{{ site.atb_release_version }} --net=host apitestbase/apitestbase:{{ site.atb_release_version }}
```

Notice: Replace `/data/folder/on/host` with your own value, so that your data can be retained on the host machine when the container is deleted. For example (Windows host):

```
docker run -d -t -v C:\Users\zheng\AppData\Roaming\ApiTestBaseDocker:/atb/data --name apitestbase-{{ site.atb_release_version }} --net=host apitestbase/apitestbase:{{ site.atb_release_version }}
```

If not using host networking
```
// Windows or macOS host
docker run -d -t -v /data/folder/on/host:/atb/data --name apitestbase-{{ site.atb_release_version }} -p 8090:8090 apitestbase/apitestbase:{{ site.atb_release_version }}

// Linux host
docker run -d -t -v /data/folder/on/host:/atb/data --add-host=host.docker.internal:host-gateway --name apitestbase-{{ site.atb_release_version }} -p 8090:8090 apitestbase/apitestbase:{{ site.atb_release_version }}
```

Once the container is running, open `http://localhost:8090/ui` in a Chrome browser on the host machine.

{% endtab %}

{% tab install Java %}

Download `apitestbase-{{ site.atb_release_version }}-allos-nojre.zip` from the [release page](https://github.com/apitestbase/apitestbase-release/releases/tag/{{ site.atb_release_version }}){:target="_blank"}.

This build is for running ATB on any OS where `Java 21+` has been installed. To run the ATB, extract the downloaded zip, and run the `start.bat` (on Windows) or `start.sh` (on macOS or Linux).

It is typically used in CI/CD pipeline where you call ATB's own API (with HTTP command line tool like `curl`) for API test automation. Open `http://localhost:8090/apidoc` for Swagger UI.

You can also use it (on page `http://localhost:8090/ui`) for requests or test cases creation, but it opens a terminal window which might not be the best UX.

{% endtab %}

{% endtabs %}

## Ad Hoc Invocation
Suppose you want to invoke a REST API.

Right click anywhere on the left side pane, select `New Request` > `HTTP`, give it a name and press Enter. The request is created.

In the request edit view, under the `Invocation` tab, enter a URL like `http://localhost:8090/api/articles` (an API Test Base bundled API) and click the `Invoke` button.

![Ad Hoc HTTP Invocation](../../screenshots/basic-use/ad-hoc-http-invocation.png)

The request, as well as all the data you create in API Test Base, is automatically persisted on your local machine. No Save button.

`Docker users`: if not using host networking, to enable ATB docker container to call your API on the host machine, use `host.docker.internal` as hostname in the request URL.
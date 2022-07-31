---
title: Quick Start
permalink: /docs/en/quick-start
key: docs-quick-start
---
## Download
Download the latest API Test Base release [apitestbase-{{ site.atb_release_version }}-dist.zip](https://github.com/apitestbase/apitestbase/releases/download/{{ site.atb_release_version }}/apitestbase-{{ site.atb_release_version }}-dist.zip). Release note can be found [here](https://github.com/apitestbase/apitestbase/releases).

Unpack the zip file. The created folder will be referred to as `<APITestBase_Home>` hereafter.

Alternatively, you can also [build API Test Base by yourself](/docs/en/build-api-test-base-by-yourself).

## Prerequisite: Java 11+
[Install JRE (Java Runtime Environment) or JDK (Java Development Kit)](/docs/en/install-java) 11+ if it is not already on your machine.

To verify Java is on your machine, just open a command line window/terminal and run

```
java -version
```

You should see something like

```
openjdk version "17.0.4" 2022-07-19
OpenJDK Runtime Environment Temurin-17.0.4+8 (build 17.0.4+8)
OpenJDK 64-Bit Server VM Temurin-17.0.4+8 (build 17.0.4+8, mixed mode, sharing)
```

## Launch
{% tabs launch %}

{% tab launch Windows %}
Open a PowerShell window, cd to `<APITestBase_Home>` and run below command

```
java "-Djava.net.useSystemProxies=true" -jar apitestbase-{{ site.atb_release_version }}.jar server config.yml
```

Alternatively you can simply double click `<APITestBase_Home>\start.bat` from File Explorer.
{% endtab %}

{% tab launch MacOS or Linux %}
Open a terminal, cd to `<APITestBase_Home>` and run below command

```
java -Djava.net.useSystemProxies=true -jar apitestbase-{{ site.atb_release_version }}.jar server config.yml
```
{% endtab %}

{% endtabs %}

Once the application is successfully launched, there will be a log like below displayed in the command line output

```
API Test Base started with UI address http://localhost:8090/ui
```

Open a web browser (Google Chrome preferred), and go to the UI address.

## Ad Hoc Invocation
If you only want to invoke an API and see its response, just need to create a test step in a new or existing test case.

To create a new test case, right click on a folder in the tree, select Create Test Case and give it a name.

![New Ad Hoc Test Case](../../screenshots/basic-use/new-ad-hoc-test-case.png)

Suppose you want to invoke a REST API. On the right pane of the screen, i.e. the test case edit view, under the Test Steps tab, click Create dropdown button and select HTTP Step. The test step edit view opens.

Under the Basic Info tab, set the test step name. Under the Invocation tab, select Method like `GET`, set URL like `http://localhost:8090/api/articles` (an API Test Base bundled API) and click Invoke button.

![Ad Hoc HTTP Invocation](../../screenshots/basic-use/ad-hoc-http-invocation.png)
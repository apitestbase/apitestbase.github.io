---
title: Getting Started
permalink: /docs/en/getting-started
key: docs-getting-started
---
## Download
Download the [latest API Test Base release](https://github.com/apitestbase/apitestbase/releases/latest/download/apitestbase-dist.zip) and unpack it. The created folder will be referred to as `<APITestBase_Home>` hereafter.

Alternatively, you can also [build API Test Base by yourself](https://github.com/apitestbase/apitestbase/wiki/Build-API-Test-Base-by-Yourself).

## Launch
Prerequisites: JRE (Java SE Runtime Environment) or JDK 8+.

Open a command line window, cd to `<APITestBase_Home>` and run below command

`java -Djava.net.useSystemProxies=true -jar apitestbase-<version>.jar server config.yml`

On Windows, alternatively you can simply run `<APITestBase_Home>\start.bat`.

Once the application is successfully launched, there will be a log like below displayed in the command line output

`API Test Base started with UI address http://localhost:8090/ui`

Open a web browser (Google Chrome preferred), and go to the UI address.

## Ad Hoc Invocation
If you only want to invoke an API and see its response, just need to create a test step in a new or existing test case.

To create a new test case, right click on a folder in the tree, select Create Test Case and give it a name.

![New Ad Hoc Test Case](../../screenshots/basic-use/new-ad-hoc-test-case.png)

Suppose you want to invoke a REST API. On the right pane of the screen, i.e. the test case edit view, under the Test Steps tab, click Create dropdown button and select HTTP Step. The test step edit view opens.

Under the Basic Info tab, set the test step name. Under the Invocation tab, select Method like `GET`, set URL like `http://localhost:8090/api/articles` (an API Test Base bundled API) and click Invoke button.

![Ad Hoc HTTP Invocation](../../screenshots/basic-use/ad-hoc-http-invocation.png)

## Maintain
The first time you launch the application, two new folders are created automatically under `<APITestBase_Home>`.

    database - where system database and a sample database are located. Both are H2 databases. 
        System database is used to store all test cases, environments, endpoints, etc. you create using API Test Base.
        Sample database is for you to play with API Test Base basic features such as REST API testing or database testing. An Article table is in it.
    
    logs - where API Test Base application runtime logs are located.

**It is highly recommended that you back up `<APITestBase_Home>/database` folder regularly.** Remember to shut down the application before backing up.

To shut down the application

    On Windows: Ctrl + C
    
    On Linux/Unix: kill -SIGINT <pid>

You can tune API Test Base application to suit your runtime needs by changing contents of the config.yml under `<APITestBase_Home>`. For example, you can change the UI port number through the property `server > applicationConnectors > port` in config.yml. Refer to [Dropwizard doc](https://www.dropwizard.io/1.3.4/docs/manual/configuration.html) for more information. Re-launch API Test Base for the changes to take effect.

To move API Test Base to a different folder or computer/VM, just shut down the application, copy the whole `<APITestBase_Home>` folder over, and launch the application from there.

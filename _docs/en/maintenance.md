---
title: Maintenance
permalink: /docs/en/maintenance
key: docs-maintenance
---
API Test Base application stores data in a folder called `<APITestBase_Data>`. Following is the value:

{% tabs data-folder %}

{% tab data-folder Windows %}

```
%USERPROFILE%\AppData\Roaming\ApiTestBase
```

{% endtab %}

{% tab data-folder Mac OS %}

```
~/Library/Application Support/API Test Base
```

{% endtab %}

{% tab data-folder Docker %}

The `/data/folder/on/host` you provided in the `docker run` command.

{% endtab %}

{% endtabs %}

The first time you launch the application, below new folders are created automatically under the `<APITestBase_Data>` folder.

```
database - where system database and a sample database are located. Both are H2 databases. 
    System database is used to store all test cases, environments, endpoints, etc. you create using API Test Base.
    Sample database is for you to play with API Test Base basic features such as REST API testing or database testing. An Article table is in it.
   
logs - where API Test Base application runtime logs are located.
   
lib - where you put external libraries when needed.
```

It is recommended that you back up `<APITestBase_Data>/database` folder regularly. Remember to exit the application before backing up.

You can tune API Test Base application to suit your runtime needs by changing contents of `<APITestBase_Data>/config.yml`.

For example, you can change the UI port number through the property `server > applicationConnectors > port` in config.yml. Refer to [Dropwizard doc](https://www.dropwizard.io/en/stable/manual/configuration.html){:target="_blank"} for more information. Re-launch API Test Base for the changes to take effect.

---
title: Maintain
permalink: /docs/en/maintain
key: docs-maintain
---
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

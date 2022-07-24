---
title: Build API Test Base by Yourself
permalink: /docs/en/build-api-test-base-by-yourself
key: docs-build-api-test-base-by-yourself
---
Prerequisites: JDK (Java Development Kit) 11+, Apache Maven 3.3+.

Download the latest API Test Base release source code from [here](https://github.com/apitestbase/apitestbase/releases/latest) to your local machine. Extract it, cd to the root directory (in which there is README.md), and run below Maven command

`mvn clean package -P prod`

An `apitestbase-assembly/dist` folder is created containing the build.

Notes:
* If this is the first time you build ATB, it could take 10 minutes (depending on your network speed) for Maven to download all the dependencies. From the second time, you should see the build time decreased to around 30 seconds, as the dependencies are already in your Maven local repository.
* Build from release source code is recommended. Build from snapshot source code might have breaking changes which couldn't work with existing ATB system database, and it couldn't be easily upgraded to newer release version in future.
* Apache Maven 3.8.5 seems not working for building ATB.
  

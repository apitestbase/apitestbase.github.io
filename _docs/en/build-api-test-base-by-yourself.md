---
title: Build API Test Base by Yourself
permalink: /docs/en/build-api-test-base-by-yourself
key: docs-build-api-test-base-by-yourself
---
Prerequisites: JDK (Java SE Development Kit) 8+, Apache Maven 3.3+.

Download the latest Iron Test source code release from [here](https://github.com/zheng-wang/irontest/releases) to your local machine. Extract it, cd to the root directory (in which there is README.md), and run below Maven command

`mvn clean package -P prod`

An `irontest-assembly/dist` folder is created containing the files and folders.

Notice that if this is the first time you build Iron Test, it could take 10 minutes (depending on your network speed) for Maven to download all the dependencies. From the second time, you should see the build time decreased to around 30 seconds, as the dependencies are already in your Maven local repository.
  

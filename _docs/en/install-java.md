---
title: Install Java
permalink: /docs/en/install-java
key: docs-install-java
---
Here are some simple instructions for installing Open JRE/JDK built by Eclipse Temurin, which is free and popular.

You can also install JRE/JDK built by another organization (refer to the organization's documentation).

## Download
Open [Eclipse Temurin web site](https://adoptium.net/temurin/releases) in your browser.

Parameters:
* Operating System: choose your own.
* Architecture: the CPU architecture. Choose your own.
* Package Type: choose JRE if you only want to run API Test Base. Choose JDK if you want to [build API Test Base by yourself](/docs/en/build-api-test-base-by-yourself) as well.
* Version: latest LTS version is recommended. Refer to the [Release Table](https://en.wikipedia.org/wiki/Java_version_history#Release_table) for available versions.

Download your desired JRE/JDK after choosing the parameter values. For easy installation, .msi file is recommended for Windows user, and .pkg file is recommended for MacOS user.

## Install
{% tabs java %}

{% tab java Windows %}
Double click the downloaded .msi file to install Java. Keeping all default settings during the wizard run is the simplest way.
{% endtab %}

{% tab java MacOS %}
Click the downloaded .pkg file to install Java.
{% endtab %}

{% tab java Linux %}
Extract the downloaded package to a folder. Here we refer to the folder as `<Java_Home>`. Add `<Java_Home>/bin` to your `Path` environment variable. 
{% endtab %}
{% endtabs %}

## Verify
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
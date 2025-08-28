---
title: Home
layout: article
permalink: /
key: home
pageview: true
show_title: false
tags:     # Not sure why. Unlike other article pages, here can't use 'tags: tag1 tag2, ...', because it will cause 'tags[0]' in _includes/article-info.html not working, hence not rendering the 'keywords' metadata.
  - home
  - integration-unit-testing
  - integration-testing
  - test-automation 
  - api-testing
  - database-testing
  - http-stub
  - http-testing
  - soap-testing
  - mq-testing
  - ace-testing
  - mock-service
  - data-driven-testing
  - amqp-testing
  - ftp-testing
  - sftp-testing
  - activemq-testing
  - solace-testing
  - mqtt-testing
  - jms-testing
---
API Test Base is a free tool for integration testing a variety of APIs. It is suitable for Integration, ESB and Microservices testing.

Currently supported API types: HTTP, SOAP, Relational databases (Oracle, SQL Server, PostgreSQL, H2, etc.), JMS (ActiveMQ, Solace), FTP(S), SFTP, AMQP, MQTT, IBM MQ, IBM App Connect Enterprise (ACE; formerly IIB).

API Test Base enables you to create and maintain automated API test cases at lower cost.

<div style="text-align:center"><a class="button button--outline-primary button--pill" href="/docs/en/quick-start">Quick Start</a></div>

Feature highlights
* Support a lot of API types. Not just HTTP.
* No code. Low code.
* Offline first. All your data is stored on your local machine. No internet connection is required to use the tool.
* Standalone requests, and plain old test cases.
* HTTP stubs (mock servers), built-in data driven testing, endpoints management, placeholders in assertions, etc.
* Support Docker.
* Support VCS (like Git) based team collaboration.

![UI Glance](../../screenshots/ui-glance.png)

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
  - iib-testing
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
API Test Base is an open source tool for integration testing a variety of APIs. It is 100% free and suitable for Integration, ESB, Microservices and SOA testing.

Supported API types: HTTP, SOAP, Relational databases (Oracle, SQL Server, H2), JMS (ActiveMQ, Solace), FTP(S), SFTP, AMQP, MQTT, IBM MQ, IBM Integration Bus (IIB).

<div style="text-align:center"><a class="button button--outline-success button--pill" href="/docs/en/quick-start">Quick Start</a></div>

The tool
* has GUI, saving user programming skills. Developers also have better experience using GUIed tool, as writing, reading and maintaining code are more brain power draining.
* intends to provide a platform that enables integrating testing capabilities for all types of APIs.
* produces test cases easily readable by technical and non-technical people, and intends to enable sharing test cases across the entire organization.
* intends to replace traditional unit testing in the API ecosystem with integration unit testing. [Here](https://medium.com/@zhengwang666/integration-unit-testing-683fbf995c43) is the theory.

![UI Glance](../../screenshots/ui-glance.png)

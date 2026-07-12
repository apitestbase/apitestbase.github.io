---
title: MQTT Request
permalink: /docs/en/mqtt-request
key: docs-mqtt-request
---
MQTT request is used to publish a message onto an MQTT topic.

An MQTT request can be used for ad hoc message publishing, replacing a separate MQTT client tool, or in a test case to publish a message that triggers the API under test, whose side effects can then be verified in the same test case — see [Test APIs triggered by queue messages and file drops](/#test-apis-triggered-by-queue-messages-and-file-drops).

MQTT is implemented by all major messaging providers. MQTT request has been tested against ActiveMQ, RabbitMQ (with the MQTT plugin enabled) and Solace, all without security enabled.

The MQTT client library is bundled with API Test Base, so no setup is needed.

Actions available: **Publish**.

Example MQTT server URI: `tcp://localhost:1883`.

## ActiveMQ as MQTT Service Provider
The topic string is the ActiveMQ topic name.

## RabbitMQ as MQTT Service Provider
When publishing to RabbitMQ, the message is published to the topic exchange `amq.topic` (the RabbitMQ MQTT plugin's default), and the topic string is translated into the routing key: MQTT uses `/` as the topic level separator while AMQP 0-9-1 uses `.`, so topic string `cities/london` becomes routing key `cities.london`. Bind a queue to `amq.topic` with a matching binding key for the message to land in the queue.

Avoid dots in the topic string, as they do not translate as expected.

## Solace as MQTT Service Provider
The topic string is the Solace topic.
---
title: AMQP Request
redirect_from: /docs/en/amqp-test-step
permalink: /docs/en/amqp-request
key: docs-amqp-request
---
AMQP request is used to operate on an AMQP node (such as a queue or topic).

An AMQP request can be used for ad hoc message sending, replacing the broker's management console or a throwaway client script, or in a test case to send a message that triggers the API under test, whose side effects can then be verified in the same test case — see [Test APIs triggered by queue messages and file drops](/#test-apis-triggered-by-queue-messages-and-file-drops).

AMQP request only supports AMQP 1.0. It has been tested against ActiveMQ 5.17.x, RabbitMQ 3.13.1 (with the AMQP 1.0 plugin enabled), and IBM MQ 8 and 9, all without security enabled.

A generic AMQP client library is bundled with API Test Base, so no setup is needed.

Actions available: **Send**.

Example AMQP service URI: `amqp://localhost:5672`.

## ActiveMQ as AMQP Service Provider
Set target address to `<queue name>` or `queue://<queue name>` for sending messages to queue.

Set target address to `topic://<topic string>` for sending messages to topic.

## RabbitMQ as AMQP Service Provider
When sending messages to RabbitMQ, the target address is interpreted as described in the [RabbitMQ AMQP 1.0 plugin documentation](https://github.com/rabbitmq/rabbitmq-amqp1.0#routing-and-addressing). Some more examples with detailed explanation:

- `/amq/queue/queue1`: send message to default exchange with routing key queue1; queue queue1 must be durable and already exist; the send action does not create the queue if it does not exist.
- `queue2`: send message to default exchange with routing key queue2; queue queue2 must be transient; the send action creates the queue if it does not already exist.
- `/topic/news`: send message to exchange amq.topic with routing key news; amq.topic is the default topic exchange created when creating a RabbitMQ node; bind a queue to amq.topic with routing key news for the message to land in the queue.

## IBM MQ as AMQP Service Provider
When sending messages to IBM MQ, the target address is a topic string.

Up to IBM MQ 9.2 LTS, IBM MQ supports Publish/Subscribe style AMQP only: it is impossible to send the message directly to a queue, and you have to create a subscription to the topic for the message to land in a queue. Refer to [Configuring AMQP clients to interact directly with applications on IBM MQ queues](https://www.ibm.com/docs/en/ibm-mq/9.1?topic=tacm-configuring-amqp-clients-interact-directly-applications-mq-queues) for more details.

From IBM MQ 9.3, point-to-point style AMQP (sending messages directly to a queue) is also supported, but this has not been tested with API Test Base.
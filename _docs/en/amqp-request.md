---
title: AMQP Request
redirect_from: /docs/en/amqp-test-step
permalink: /docs/en/amqp-request
key: docs-amqp-request
---
AMQP request is used to operate on an AMQP node (like queue).

AMQP request only supports AMQP 1.0. It was tested against ActiveMQ 5.17.x, RabbitMQ 3.13.1 (with AMQP 1.0 plugin enabled), as well as IBM MQ 8 and 9, with no security.

Actions available: **Send**.

Example AMQP service URI: `amqp://localhost:5672`.

### ActiveMQ as AMQP Service Provider
Set target address to `<queue name>` or `queue://<queue name>` for sending messages to queue.

Set target address to `topic://<topic string>` for sending messages to topic.

### RabbitMQ as AMQP Service Provider
When sending messages to RabbitMQ, the target address is interpreted [here](https://github.com/rabbitmq/rabbitmq-amqp1.0#routing-and-addressing). Some more examples with detailed explanation:

- `"/amq/queue/queue1"`: send message to default exchange with routing key queue1; queue queue1 must be durable and already existing; the send action does not create the queue if it is not existing.
- `"queue2"`: send message to default exchange with routing key queue2; queue queue2 must be transient; the send action creates the queue if it is not already existing.
- `"/topic/news"`: send message to exchange amq.topic with routing key news; amq.topic is the default topic exchange created when creating a RabbitMQ node; bind a queue to amq.topic with routing key news for the message to land in the queue.

### IBM MQ as AMQP Service Provider
When sending messages to IBM MQ, the target address is a topic string.

IBM MQ only supports Publish/Subscribe style AMQP, and you have to create a subscription to the topic for the message to land in a queue. It is impossible to send the message directly to a queue. Refer to [this post](https://developer.ibm.com/messaging/2018/12/18/configuring-amqp-clients-to-interact-with-applications-that-put-to-or-get-from-mq-queues/) for more details.

---
title: JMS Request
redirect_from: /docs/en/jms-test-step
permalink: /docs/en/jms-request
key: docs-jms-request
---
JMS request is used to operate on a JMS queue or topic, like sending messages to a queue.

Supported JMS providers: **ActiveMQ, Solace**.

Actions available: **Send (and optionally Also Receive), Browse Queue, Publish, Clear Queue, Check Queue Depth**.

## Endpoint Details
To operate on a JMS queue or topic, some endpoint parameters are needed.

![Endpoint Details](../../screenshots/jms/endpoint-details.png)

## Send Action
Send action is used to send a JMS message into a queue.

You can do one-way/fire-and-forget style invocation, i.e. send message to queue and done.

You can also do request-reply style invocation, i.e. send message to request queue and receive another message from reply queue. For example:

![Send Message and Also Receive](../../screenshots/jms/send-message-and-also-receive.png)

Message body can be XML, JSON, or any other text format.

Click `Do` button to Send the message and receive the reply.

## Browse Action
Browse action is used to read a message from a queue without deleting the message.

You can browse the queue by message index or by a substring (in message body).
 
![Browse Queue by Substring](../../screenshots/jms/browse-queue-by-substring.png)

Click `Do` button to read the message.

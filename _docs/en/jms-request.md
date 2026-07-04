---
title: JMS Request
redirect_from: /docs/en/jms-test-step
permalink: /docs/en/jms-request
key: docs-jms-request
---
JMS request is used to operate on a JMS queue or topic, like sending messages to a queue.

A JMS request can be used for ad hoc queue or topic operations, replacing the JMS provider's own console or client tool, or in a test case to send a message that triggers the API under test, to browse the API's output queue and verify the output message, or both — see [Test APIs triggered by queue messages and file drops](/#test-apis-triggered-by-queue-messages-and-file-drops).

Supported JMS providers: **ActiveMQ, Solace**.

The ActiveMQ jars are bundled with API Test Base. To use the Solace provider, set up the Solace jars first as described in [Interact with Other Systems](/docs/en/interact-with-other-systems#solace).

Actions available: **Send (and optionally Also Receive), Browse Queue, Clear Queue, Check Queue Depth, Publish**.

## Endpoint Details
To operate on a JMS queue or topic, some endpoint parameters are needed.

![Endpoint Details](../../screenshots/jms/endpoint-details.png)

## Send Action
Send action is used to send a JMS message into a queue.

You can do one-way/fire-and-forget style invocation, i.e. send message to queue and done.

<div class="video-container">
    <iframe width="560" height="315" src="https://www.youtube.com/embed/EZzxLVzw-vo?si=GUVb6urvdscrGHxU" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
</div>

You can also do request-reply style invocation, i.e. send message to request queue and receive another message from reply queue. For example:

![Send Message and Also Receive](../../screenshots/jms/send-message-and-also-receive.png)

<div class="video-container">
    <iframe width="560" height="315" src="https://www.youtube.com/embed/n_aMtRdIMr4?si=uMpDZl1hQjm4O_nX" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
</div>

Message body can be XML, JSON, or any other text format.

Click `Do` button to send the message and, in the case of request-reply style invocation, receive the reply.

## Browse Action
Browse action is used to read a message from a queue without deleting the message.

You can browse the queue by message index or by a substring (in message body).

![Browse Queue by Substring](../../screenshots/jms/browse-queue-by-substring.png)

Click `Do` button to read the message.
---
title: MQ Request
redirect_from: /docs/en/mq-test-step
permalink: /docs/en/mq-request
key: docs-mq-request
---
MQ request is used to operate on an IBM MQ queue or topic.

Actions available: **Enqueue, Dequeue, Publish, Clear Queue, Check Queue Depth**.

## Endpoint Details
To operate on an IBM MQ queue or topic, some endpoint parameters are needed for API Test Base to connect to the queue manager.

![Endpoint Details](../../screenshots/mq/endpoint-details.png)

## Enqueue Action
Enqueue action is used to `PUT` a message into a queue. You can provide the message in two ways.

### Provide message by entering text
![Enqueue MQ Message From Text](../../screenshots/mq/enqueue-message-from-text.png)

Text can be XML, JSON, or any other text format.

Click `Do` button to PUT the message into the queue.

You can also include an **MQRFH2 header**, with one or more MQRFH2 folders, in the message, like below. Notice that each RFH2 folder must be a valid XML document.

    <mcd>                                //  an MQ defined/reserved RFH2 folder
        <Set></Set>
        <Type></Type>
        <Fmt></Fmt>
        <Msd>xmlnsc</Msd>
    </mcd>   

    <customFolder1>                      //  a custom RFH2 folder
        <field1>value1</field1>
        <field2>value2</field2>
    </customFolder1>

![Enqueue MQ Message From Text with MQRFH2 Header](../../screenshots/mq/enqueue-message-from-text-with-rfh2-header.png)

### Provide message by uploading a file

![Enqueue MQ Message From File](../../screenshots/mq/enqueue-message-from-file.png)

This enables you to PUT a message with any format, no matter binary or text.

Message in the file can contain MQMD header which will be recognized. If there is no MQMD header, one will be generated.

Again, click `Do` button to PUT the message into the queue.

## Dequeue Action
Dequeue action is used to `GET` a message from a queue. You can create assertions against the returned message body as well as RFH2 header.

Click `Do` button to get the message. Click the `Assertions` button to open assertions panel and add/edit/verify assertions.
  
![Dequeue MQ Message](../../screenshots/mq/dequeue-message.png)

Currently the dequeued message body is assumed to be text.

## Publish Action
Publish action is used to publish a message onto a topic. Same as Enqueue action, you can provide the message in two ways. Here is the text way:

![Publish MQ Message From Text](../../screenshots/mq/publish-message-from-text.png)

Click `Do` button to publish the message onto the topic.
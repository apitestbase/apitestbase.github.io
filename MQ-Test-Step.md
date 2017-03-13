MQ test step is normally used in combination with other test steps such as SOAP, IIB, etc. to create automated test cases. However, you can also use it to manually operate on an MQ queue or topic. (Actually, most types of test steps in Iron Test can be used to individually/manually invoke target API)

Actions available in the MQ test step: **Clear Queue, Check Queue Depth, Enqueue, Dequeue, Publish**.

## Endpoint Details
To operate on an MQ queue, some parameters are needed for Iron Test to connect to the queue manager.

[![Endpoint Details](https://github.com/zheng-wang/irontest/blob/master/screenshots/mq/endpoint-details.png)](https://github.com/zheng-wang/irontest/blob/master/screenshots/mq/endpoint-details.png)

## Enqueue Action
Enqueue action is used to PUT a message into a queue. You can provide the message in two ways.

### Provide message by entering text
[![Enqueue MQ Message From Text](https://github.com/zheng-wang/irontest/blob/master/screenshots/mq/enqueue-message-from-text.png)](https://github.com/zheng-wang/irontest/blob/master/screenshots/mq/enqueue-message-from-text.png)

Click the Do button to PUT the message into the queue.

You can also include an **MQRFH2 header**, with one or more MQRFH2 folders, in the message. For example, you can add RFH2 folders like below.

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

[![Enqueue MQ Message From Text with MQRFH2 Header](https://github.com/zheng-wang/irontest/blob/master/screenshots/mq/enqueue-message-from-text-with-rfh2-header.png)](https://github.com/zheng-wang/irontest/blob/master/screenshots/mq/enqueue-message-from-text-with-rfh2-header.png)

### Provide message by uploading a file

[![Enqueue MQ Message From File](https://github.com/zheng-wang/irontest/blob/master/screenshots/mq/enqueue-message-from-file.png)](https://github.com/zheng-wang/irontest/blob/master/screenshots/mq/enqueue-message-from-file.png)

Message in the file can contain MQMD header which will be recognized by Iron Test. If there is no MQMD header, Iron Test will generate one.

Again, click the Do button to PUT the message to the queue.

## Dequeue Action
Dequeue action is used to GET a message from a queue. You can assert the returned message content (currently only XML message is supported).

Click Do button to get the message, and click Verify button to verify the assertion.
  
[![Dequeue MQ Message](https://github.com/zheng-wang/irontest/blob/master/screenshots/mq/dequeue-message.png)](https://github.com/zheng-wang/irontest/blob/master/screenshots/mq/dequeue-message.png)
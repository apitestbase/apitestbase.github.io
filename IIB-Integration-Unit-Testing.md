If you have been familiar with the SOAP Web Service Testing and Endpoints Management, it will be intuitive for you to do IIB integration unit testing using Iron Test.

Here is a sample scenario. 

The message flow under test (Flow1) has a SOAP Input node to receive SOAP request, and an MQ Output node to output the message to an MQ local queue. 

[![Flow1 Code](https://github.com/zheng-wang/irontest/blob/master/screenshots/iib/flow1-code-diagram.png)](https://github.com/zheng-wang/irontest/blob/master/screenshots/iib/flow1-code-diagram.png)

The queue is a 'joint' queue as there is a downstream message flow (Flow2) listening to it, like shown below.

The primary way to integration unit test Flow1 is to provide input to it and examine its output. Now there is a problem. Before we get a chance, the output message produced by Flow1 is immediately picked up by Flow1, i.e. we won't be able to examine Flow1's output message here.

A critical thing to do for IIB integration unit testing is to `isolate the message flow under test`. In our scenario, we need to isolate Flow1. There are several methods to do so.
1. Redesign Flow1 to use an alias queue as its output queue which points to the 'joint' queue (make this a standard in the team so that the next time we won't need to redesign another message flow to test). Before testing Flow1, (manually or automatically) modify the alias queue to point to a stub local queue for Flow1.
2. During Flow1 deployment, use baroverride to modify the queue name property on the MQ Output node in Flow1 to be a stub local queue.
3. Stop the downstream message flow (Flow2), so that no one (other than the test case) is getting messages from the 'joint' queue.
4. Other?

Method 1 is my favorite as it is simple.
   
Based on the isolation, a positive test case for Flow1 would have these steps.

    Setup - clear stub output queue
    Invoke web service with a valid SOAP request and assert successful SOAP response
    Wait for message processing completion
    Check stub output queue depth equals 1
    Dequeue message from stub output queue and assert expected message body    

The step 'Wait for message processing completion' is to ensure that Flow1 finishes all the work processing the input message, including outputting the message to the MQ queue. The web service returning SOAP response does not necessarily mean Flow1 finishes the processing.

Hopefully you are able to DIY now. The result test case looks like below

[![Web Service to Queue](https://github.com/zheng-wang/irontest/blob/master/screenshots/iib/ws-to-queue.png)](https://github.com/zheng-wang/irontest/blob/master/screenshots/iib/ws-to-queue.png)

Notice that the isolation is only needed in integration unit testing environment. Other environment such as ST (System Testing) or SIT (System Integration Testing) environment may not need it as the testing strategy is different. On the other hand, configuring a message flow or queue differently in different environments is quite common in IIB project.

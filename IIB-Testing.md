If you have been familiar with the SOAP Web Service Testing and Endpoints Management, it will be intuitive for you to do IIB Testing using Iron Test.

Here is a sample test case for (in-server) unit testing an IIB message flow. The message flow (Flow1) has a SOAPInput node to receive SOAP request, and an MQOutput node to output the message to an MQ queue. 

There is a downstream message flow (Flow2) listening to the queue, so to isolate the testing of Flow1, we need some setup steps.

    Setup - stop downstream message flow
    Setup - clear output queue
    Setup - start the message flow under test
    
Then we add below test steps to the same test case

    Invoke web service with a valid SOAP request and assert successful SOAP response
    Check output queue depth equals 1
    Dequeue message from the output queue and assert expected message body
    
Hopefully you are able to DIY now. The result test case looks like below

[![Web Service to Queue](https://github.com/zheng-wang/irontest/blob/master/screenshots/iib/ws-to-queue.png)](https://github.com/zheng-wang/irontest/blob/master/screenshots/iib/ws-to-queue.png)
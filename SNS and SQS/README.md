## SQS and SNS for a loosely coupled system

### I. Simple Queue Service
SQS is an AWS resource that allows you to create a queue that other parts of your system can access. This gives multiple benefits such as separating two distinct parts and allows multiple components put items into the queue or take them out. This is a convenient way to allow the system to be scaleable in just the areas that need to scale.

When you create a SQS queue there are a variety of options that can be set. In my case I needed to lengthen the visibility timeout because my process could take minutes to complete. I also chose to set a delivery delay as my process needed a period of time before it was ready to execute.

Using a SQS is a three part process:
1. Putting a message into the queue.
1. Retrieving a message from the queue. This is done with `receive_message(QueueUrl)` with other optional parameters like the number of messages to be received or how long it should wait to receive a message. If the queue is nearly empty it may not receive anything out of the queue unless you increase the polling time or just are willing to poll repeatedly.
1. Deleting the message after it has been processed. This is done with `delete_message(QueueUrl, ReceiptHandle)`. The ReceiptHandle comes with the original message to identify the message to be deleted. The reason it needs to be explicitly deleted is that in distributed application if a process fails to use the message then the queue will allow the message to be retrieved and processed.


### II. Simple Notification Service

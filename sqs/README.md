# Simple Queue Service

FIFO: `5 -> 4 -> 3 -> 2 -> 1`. Good and slow.
Regular: `4 -> 3 -> 3 -> 5 -> 2 -> 1`. Not perfect, but fast.

* Decrease component coupling
* Reliability - message is 100% processed
* Scalability

*You can get the same message twice with regular queue*

*There is a dead letter queue - queue with messages that failed several times*

## SQS vs SNS

SNS - to all
SQS - to one

## Create queue

`SQS -> Create queue` 

Name | Queue Type| Message Available | Message in Flight
---- | --------- | ----------------- | -----------------
tc-my-test-sq | Standard | 150 | 1


## Example

Sender:

```
AmazonSQS sqs = AmazonSQSClientBuilder()
  .standard()
  .withRegion(US_EAST_1)
  .build();

String url = sqs.createQueue("tc-my-test-sq").getQueueUrl();

for (int i = 0; i < 1100: ++i) {
  sqs.sendMessage(new SendMessageRequest(...)
    .withQueueUrl(queueUrl)
    .withMessageBody("123"));

}
```

Receiver:

```
AmazonSQS sqs = AmazonSQSClientBuilder()
  .standard()
  .withRegion(US_EAST_1)
  .build();

sqs.createQueue(QUEUE).getQueueUrl();

while(true) {
  ReceiveMessageRequest request = new ReceiveMessageRequest()
    .withQueueUrl(queueUrl)
    .withMaxNumberOfMessages(1)
    .withWaitTimeSeconds(20);

  ReceiveMessageResult result = sqs.receiveMesage(request);
   
  if (result.getMessages().size() > 0) {
    message.getBody();
     
    sqs.deleteMessage(new DeleteMessageRequest().withQueueUrl(queueUrl).withReceiptHandle(result));
  }
} 
```


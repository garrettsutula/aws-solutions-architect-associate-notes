# Simple Queue Service (SQS)
[SQS FAQ](https://aws.amazon.com/sqs/faqs/)

Web service for queueing to support **decoupled**, event-driven architecture. 

Queues are fail-safe, similar to RabbitMQ. Messages can contain up to 256KB of text, messages higher than that are stored in S3. SQS acts as a buffer between producers and consumers.

SQS is pull-based, not push-based. SQS supports long-polling to delay response until a message arrives or a timeout occurs.

Messages can be kept in the queue from 1 minute to 14 days, the default retention is 4 days.

**Visiblity timeout** is the amount of time a message has to be processed before failure is assumed and it re-appears in the queue. This can result in the same message being delivered twice (i.e. visibility timeout not long enough). **Maximum** visibility timeout is `12 hours`.

There are two types of queues:
- Standard Queues
- FIFO queues

## Standard Queues
Nearly unlimited transactions per second, guarantees that a message is delivered at least once. 

Ocassionally, because of highly-distributed architecutre to support the high throughput, more than one copy of a message may be delivered out of order.

Standard queues have best-effort ordering, generally messages are delivered in the same order they're sent.

Applications should be designed to be resiliant against:
- Delivery of the same message more than once
- Out of order delivery

## FIFO Queues
First-in, first-out and **exactly** once processing. Slower, earlier messages must be processed and acknowledged prior to processing the next message.

Limited to 300 transations per second, otherwise all the same capabilities as standard queues.

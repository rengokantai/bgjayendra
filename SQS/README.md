##Overview
A queue is a temporary repository for messages that are 
awaiting processing and acts as a buffer between the component producer and the consumer

###Features & Key Points
- Redundant infrastructure
  - offers reliable and scalable hosted queues for storing messages
  - is engineered to always be available and deliver messages
  - provides ability to store messages in a fail safe queue
  - highly concurrent access to messages
  
- At-Least-Once delivery
  - ensures delivery of each message at least once
  - stores copies of the messages on multiple servers for redundancy and high availability. 
  On rare occasions, one of the servers storing a copy of a message might be unavailable when you receive or delete the message. 
  If that occurs, the copy of the message will not be deleted on that unavailable server, 
  and you might get that message copy again when you receive messages.
  - Applications should be designed to be idempotent with the ability to handle duplicate messahes and not be adversely affected 
  if it processes the same message more than once

- Message Sample
  - behavior of retrieving messages from the queue depends on whether short (standard) polling, 
  the default behavior, or long polling is used
  - With short polling, SQS samples only a subset of the servers (based on a weighted random distribution) and returns messages from just those servers. 
  __This means that a particular receive request might not return all your messages in the queue.__ 
  However, subsequent receive request would return the message
  - With Long polling, persists for the time specified and returns as soon as the message is available thereby reducing SQS costs and
  __time the message has to dwell in the queue__

- Batching
  - SQS allows send, receive and delete batching which helps club upto 10 messages in a single batch and you pay price for a single message
  - In addition to the lower cost, it also increases the throughput
- Order
  - makes a best effort to preserve order in messages does not guarantee first in, first out delivery of messages 
  - can be handled by placing sequencing information within the message and performing the ordering on the client side
- Coupling
  - removes tight coupling between components
  - provides ability to move data between distributed components of your applications 
  that perform different tasks __without losing messages or requiring each component to be always available__
  
  - Multiple writers and readers
    - supports multiple readers and writers interacting with the same queue as the same time
    - SQS locks the message during processing preventing it to be processed by any other consumer
  - Variable message size
    - supports message in any format upto __256KB__ of text.
    - messages larger than 256 KB can be managed using the __Amazon SQS or DynamoDB with SQS storing pointer__
  - Access Control
    - Access can be controlled for who can produce and consume messages to each queue
  
  
  

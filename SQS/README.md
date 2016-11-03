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
  - Delay Queues
    - delay queue allows the user to set a default delay on a queue such that delivery of all messages enqueued will be postponed for that duration of time
  
- PCI Complaince
  - Amazon SQS supports the processing, storage, and transmission of credit card data by a merchant or service provider, and has been validated as being compliant with Payment Card Industry (PCI) Data Security Standard (DSS).  
  




### How SQS Queues Works
- SQS allows you to create, delete queues and send and receive messages from the queue
- SQS queue retains messages for four days, by default. However, queues can configured to retain messages for up to 14 days after the message has been sent.
- Amazon SQS can delete your queue without notification if one of the following actions hasn’t been performed on it for 30 consecutive days.
- SQS allows the deletion of the queue with messages in it

#### Queue and Message Identifiers
#####Queue URLs
- Queue is identified by a unique queue name within the same aws account

##### Message ID
- Each message receives a system-assigned message ID that Amazon SQS returns to you in the SendMessage response.
- However to delete a message, __the message’s receipt handle instead of the message ID is needed__
- Message ID can be of is 100 characters max

##### Receipt Handle
- When a message is received from a queue, a receipt handle is returned with the message which is associated with the act of receving the message rather then the message itself
- Receipt handle is required, not the message id, to delete a message or to change the message visibility
- __If a message is received more than once, each time its received, a different receipt handle is assigned and the latest should be used always__


####Visibility timeout
- Behaviour
  - SQS does not delete the message once it is received by an consumer,
because the system is distributed, there’s no guarantee that the consumer will actually receive the message (it’s possible the connection could break or the component could fail before receiving the message)
- Consumer should explicity delete the message from the Queue once it is received and successfully processed
- __As the message is still available on the Queue, other consumers would be able to receive and process and this needs to be prevented__

- SQS handles the above behaviour using Visibility timeout.
- SQS blocks the visiblility of the message for the __Visibility timeout period__, which
is the time during which SQS prevents other consuming components from receiving and processing that message
- __Consumer should delete the message within the Visibility timout. If the consumer fails to delete the message before the visibility timeout expires, the message is visible again for other consumers.__

- Visibility timeout considerations
  - clock starts ticking once SQS returns the message
  - should be large enough to take into account the processing time for each of the message
  - default Visibility timeout for each Queue is __30 seconds__ and can be changed at the __Queue level__
  - when receiving messages, a special visibility timeout for the returned messages can be set without changing the overall queue timeout using the receipt handle
  - can be extended by the consumer, if the consumer thinks it won’t be able to process the message within the current visibility timeout period. SQS restarts the timeout period using the new value
  - __a message’s Visibility timeout extension applies only to that particular receipt of the message and  does not affect the timeout for the queue or later receipts of the message__

- SQS has an 120,000 limit for the number of inflight messages per queue i.e. message received but not yet deleted and any further messages would receive an error after reaching the limit




  

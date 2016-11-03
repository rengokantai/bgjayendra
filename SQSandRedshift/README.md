##SQS
### Ideal Usage patterns
- is ideally suited to any scenario where multiple application components must communicate and coordinate their work 
in a loosely coupled manner particularly producer consumer scenarios
- can be used to coordinate a multi-step processing pipeline, where each message is associated with a task that must be processed.
- enables the number of worker instances to scale up or down, and also enable the processing power of each single worker instance to scale up or down,
to suit the total workload, without any application changes.

###Anti Patterns
- Binary or Large Messages
  - SQS is suited for text messages with maximum size of 64 KB. 
If the application requires binary or messages exceeding the length, __it is best to use Amazon S3 or RDS and use SQS to store the pointer__
- Long Term storage
  - SQS stores messages for max 14 days and __if application requires storage period longer than 14 days__, Amazon S3 or other storage options should be preferred
  
- High-speed message queuing or very short tasks
  - If the application requires a very high-speed message send and receive response from a single producer or consumer,
  use of Amazon DynamoDB or a message-queuing system hosted on Amazon EC2 may be more appropriate.
  
###Performance
- is a distributed queuing system that is optimized for horizontal scalability, __not for single-threaded sending or receiving speeds__.
- A single client can send or receive Amazon SQS messages at a rate of about 5 to 50 messages per second. 
Higher receive performance can be achieved by requesting multiple messages (up to 10) in a single call.  

##Durability & Availability
- are highly durable but temporary.
- stores all messages redundantly across multiple servers and data centers.
- Message retention time is configurable on a per-queue basis, from a minimum of one hour to a maximum of 14 days.
- Messages are retained in a queue until they are explicitly deleted, 
or until they are automatically deleted upon expiration of the retention time.  

##Cost Model
- pricing is based on
  - number of requests and
  - the amount of data transferred in and out (priced per GB per month).
  
##Scalability & Elasticity
- is both highly elastic and massively scalable.
- is designed to enable a virtually unlimited number of computers to read and write a virtually unlimited number of messages at any time.
- supports virtually unlimited numbers of queues and messages per queue for any user.










## Amazon Redshift
-is a fast, fully-managed, petabyte-scale data warehouse service that makes it simple and 
cost-effective to efficiently analyze all your data using your existing business intelligence tools.
-is optimized for datasets that range from a few hundred gigabytes to a petabyte or more.
-manages the work needed to set up, operate, and scale a data warehouse, 
from provisioning the infrastructure capacity to automating ongoing administrative tasks such as backups and patching.

### Ideal Usage Pattern
- is ideal for analyzing large datasets using the existing business intelligence tools

### Anti-Pattern
- OLTP workloads
  - Redshift is a column-oriented database and more suited for data warehousing and analytics. If application involves online transaction processing, Amazon RDS would be a better choice.
- Blob data
  - For Blob storage, Amazon S3 would be a better choice with metadata in other storage as RDS or DynamoDB

### Performance
- Amazon Redshift allows a very high query performance on datasets ranging in size from hundreds of gigabytes to a petabyte or more.
- It uses columnar storage, data compression, and zone maps to reduce the amount of I/O needed to perform queries.
- It has a massively parallel processing (MPP) architecture that parallelizes and distributes SQL operations to take advantage of all available resources.
- Underlying hardware is designed for high performance data processing that uses local attached storage to maximize throughput.

### Durability & Availability
- Amazon Redshift stores __three copies__ of your data—all data written to a node in your cluster is automatically replicated to other nodes within the cluster, and all data is continuously backed up to Amazon S3.
- __Snapshots are automated, incremental, and continuous and stored for a user-defined period (1-35 days)__
- Manual snapshots can be created and are retained until explicitly deleted.
- Amazon Redshift also continuously monitors the health of the cluster and automatically re-replicates data from failed drives and replaces nodes as necessary.



###Cost Model
- has three pricing components:
  - data warehouse node hours – total number of hours run across all the compute node
  - backup storage – storage cost for automated and manual snapshots
  - data transfer
    - There is no data transfer charge for data transferred to or from Amazon Redshift outside of Amazon VPC
    - Data transfer to or from Amazon Redshift in Amazon VPC accrues standard AWS data transfer charges.



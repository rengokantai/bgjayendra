## AWS Disaster Recovery Whitepaper
### Disaster Recovery Overview
- AWS Disaster Recovery whitepaper highlights AWS services and features that can be leveraged for disaster recovery (DR) 
processes to significantly minimize the impact on data, system, and overall business operations.
- It outlines best practices to improve your DR processes, from minimal investments to full-scale availability and
fault tolerance, and describes how AWS services can be used to reduce cost and ensure business continuity during a DR event
- Disaster recovery (DR) is about preparing for and recovering from a disaster. Any event that has a negative impact on
a company’s business continuity or finances could be termed a disaster. One of the AWS best practice is to always design
your systems for failures

###Disaster Recovery Key AWS services
1. Region
  - AWS services are available in multiple regions around the globe, 
  and the DR site location can be selected as appropriate, in addition to the primary site location

2. Storage
  - Amazon S3
    - provides a highly durable (99.999999999%) storage infrastructure designed for mission-critical and primary data storage.
    - stores Objects redundantly on multiple devices across multiple facilities within a region
  - Amazon Glacier
    - provides extremely low-cost storage for data archiving and backup.
    - Objects are optimized for infrequent access, for which retrieval times of several (3-5) hours are adequate.
  - Amazon EBS
    - provides the ability to create point-in-time snapshots of data volumes.
    - Snapshots can then be used to create volumes and attached to running instances
  - Amazon Storage Gateway
    - a service that provides seamless and highly secure integration between 
    on-premises IT environment and the storage infrastructure of AWS.
  - AWS Import/Export
    - accelerates moving large amounts of data into and out of AWS by using portable storage devices for transport bypassing the Internet
    - transfers data directly onto and off of storage devices by means of the high-speed internal network of Amazon
 
3. Compute
  - Amazon EC2
    - provides resizable compute capacity in the cloud which can be easily created and scaled.
    - EC2 instance creation using Preconfigured AMIs
    - EC2 instances can be launched in multiple AZs, which are engineered to be insulated from failures in other AZs
    
4. Networking

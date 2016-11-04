##AWS Storage Gateway
- Storage Gateway is a service that connects an on-premises software appliance with cloud-based storage to provide seamless and 
secure integration between the organization’s on-premises IT environment and AWS’s storage infrastructure.
- Storage Gateway enables store data securely to the AWS cloud for scalable and cost-effective storage.
- __It provides low-latency performance by maintaining frequently accessed data on-premises while securely storing all of your data encrypted in S3__.
- For disaster recovery scenarios, it can serve as a cloud-hosted solution, together with EC2, that mirrors your entire production environment.
- Storage Gateway can be configured as
  - Gateway-cached volumes
    - Gateway-cached volumes __utilizes S3 for primary data backup__, while retaining frequently accessed data locally in a cache.
    - These volumes minimize the need to scale the on-premises storage infrastructure, while still providing applications with low-latency access to their frequently accessed data.
    - Data written to the volumes is stored in S3, 
    with only a cache of recently written and recently read data is stored locally on the on-premises storage hardware.
  - Gateway-stored volumes
    - Gateway-stored volumes stores the __complete primary data locally__, while asynchronously backing up that data to AWS.
    - These volumes provide the on-premises applications with low-latency access to their entire datasets, while providing durable, off-site backups.
    - Data written to the gateway-stored volumes is stored on the on-premises storage hardware, and asynchronously backed up to S3 in the form of EBS snapshots.
    
### Ideal Usage Patterns
- AWS Storage Gateway use cases include
  - corporate file sharing,
  - enabling existing on-premises backup applications to store primary backups on S3,
  - disaster recovery, and
  - data mirroring to cloud-based compute resources.
  
### Anti-Patterns
- Database storage
  - For Database backup or storage, __EC2 instances using EBS volumes__ are a natural choice for database storage and workloads.
  
  
### Performance
- As the Storage Gateway VM sits between the application, underlying on-premises storage and S3, the performance experienced will be dependent upon a number of factors, including the speed and configuration of the underlying local disks, the network bandwidth between the iSCSI initiator and gateway VM, the amount of local storage allocated to the gateway VM, and the bandwidth between the gateway VM and S3.  
- For gateway-cached volumes, to provide __low-latency__ read access to the on-premises applications, it’s important to provide enough local cache storage to store the recently accessed data.
- Storage Gateway efficiently uses the Internet bandwidth to speed up the upload of on-premises application data to AWS.
- Storage Gateway only uploads incremental changes (data that has changed), which minimizes the amount of data sent over the Internet.
- __AWS Direct Connect can be used to further increase throughput and reduce the network costs by establishing a dedicated network connection between the on-premises gateway and AWS.__


### Durability and Availability
- AWS Storage Gateway durably stores on-premises application data by uploading it to S3.
- S3 stores data in multiple facilities and on multiple devices within each facility.
- S3 also performs regular, systematic data integrity checks and is built to be automatically self-healing.

### Cost model
- AWS Storage Gateway has four pricing components:
  - gateway usage (per gateway per month),
  - snapshot storage usage (per GB per month),
  - volume storage usage (per GB per month), and
  - data transfer out (per GB per month).
  

### Scalability and Elasticity
- AWS Storage Gateway stores data in Amazon S3, which has been designed to offer a very high level of scalability and elasticity automatically.

### Interfaces
- AWS Management Console can be used to download the AWS Storage Gateway VM image, select between a gateway-cached or gateway-stored configuration, activate the on-premises by associating the gateway’s IP Address with your AWS account, select an AWS region, and create AWS Storage Gateway volumes and attach these volumes __as iSCSI devices to your on-premises application servers.__

## AWS Import/Export (Upgraded to Snowball)
- AWS Import/Export accelerates moving __large amounts of data into and out__ of AWS using __portable storage__ devices for transport.
- AWS transfers the data directly onto and off of storage devices using Amazon’s high-speed internal network and bypassing the Internet and can be much faster and more cost effective than upgrading connectivity.
- AWS Import/Export supports importing into several types of AWS storage, __including EBS snapshots, S3 buckets, and Glacier vaults and exporting data from S3.__


### Ideal Usage Patterns
- AWS Import/Export is ideal for __transferring large amounts of data in and out of the AWS cloud, especially in cases where transferring the data over the Internet would be too slow (a week or more) or too costly.__
- Common use cases include
  - initial data upload to AWS,
  - content distribution or regular data interchange to/from your customers or business associates,
  - transfer to Amazon S3 or Amazon Glacier for off-site backup and archival storage, and quick retrieval of large backups from Amazon S3 or Amazon Glacier for disaster recovery.
  
### Anti-Patterns
- AWS Import/Export may not be the ideal solution for data that is more easily transferred over the Internet in less than one week.

### Performance
- Each AWS Import/Export station is capable of loading data at over 100 MB per second
- Rate of the data load will be bounded by a combination of the read or write speed of the portable storage device and, for Amazon S3 data loads, the average object (file) size.

### Durability and Availability
Durability and availability characteristics of the target storage i.e. EBS, S3 or Glacier applies, after the data has been imported

### Cost Model
- AWS Import/Export has three pricing components: __a per-device fee, a data load time charge (per data-loading-hour), and possible return shipping charges (for expedited shipping, or shipping to destinations not local to that AWS Import/Export region).__
- Storage pricing applies for the destination storage, the standard Amazon EBS snapshot, Amazon S3, and Amazon Glacier request and storage pricing applies.


### Scalability and Elasticity
- Total amount of data you can load using AWS Import/Export is limited only by the capacity of the devices sent to AWS.
- For Amazon S3, individual files will be loaded as objects in Amazon S3, and may range up to __5 terabytes in size.__
- For Amazon Glacier, individual devices will be loaded as a __single archive__, and may range up to __4 terabytes in size.__
- Aggregate total amount of data that can be imported is virtually unlimited.

### Interfaces
- To upload or download data, AWS Import/Export job for each storage device shipped need to be created and submitted
- Jobs can be created using AWS CLI, AWS SDK or native REST API
- Each job request requires a manifest file, a YAML-formatted text file that contains a set of key-value pairs that supply the required information—such as your device ID, secret access key, and return address—necessary to complete the job.
- Job request is tied to the storage device through a signature file in the root directory (for Amazon S3 import jobs), or by a barcode taped to the device (for Amazon EBS and Amazon Glacier jobs).

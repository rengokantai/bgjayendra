##Overview
- Amazon Glacier is a storage service optimized for archival, infrequently used data, or “cold data.”
- Glacier is an extremely low-cost storage service that provides durable storage with security features for data archiving and backup.
- Glacier is designed to provide average annual durability of __99.999999999%__ for an archive.
- Glacier redundantly stores data in __multiple facilities and on multiple devices within each facility__. To increase durability, 
Amazon Glacier synchronously stores the data across multiple facilities before returning SUCCESS on uploading archives.
- Glacier performs regular, systematic data integrity checks and is built to be automatically self-healing.
- Glacier enables customers to offload the administrative burdens of operating and scaling storage to AWS, 
without having to worry about capacity planning, hardware provisioning, data replication, hardware failure detection 
and recovery, or time-consuming hardware migrations.
- __Glacier is a great storage choice when low storage cost is paramount,
with data rarely retrieved, and retrieval latency of several hours is acceptable.__
- __Amazon S3 should be used if applications requires fast, frequent real time access to the data__
- Glacier can store virtually any kind of data in any format.
- All data is encrypted on the server side with Glacier handling key management and key protection. It uses __AES-256__, one of the strongest block ciphers available
- Glacier allows interaction through AWS Management Console, Command Line Interface CLI and SDKs or REST based APIs.
  - management console can only be used to create and delete vaults.
  - rest of the operations to upload, download data, create jobs for retrieval need CLI, SDK or REST based APIs
  
  
## Amazon Glacier Data Model
- Amazon Glacier data model core concepts include vaults and archives and also includes job and notification-configuration resources
  - Vault
    - Vault is a container for storing archives
    - Each vault resource has a unique address, which comprises of the region the vault was created and the unique vault name 
  within the region and account for e.g. https://glacier.us-west-2.amazonaws.com/111122223333/vaults/examplevault
    - Vault allows storage of unlimited number of archives
    - Glacier supports various vault operations which are region specific
    - An AWS account can create up to __1,000 vaults per region__.
  - Archive
    - An archive can be any data such as a photo, video, or document and is a base unit of storage in Amazon Glacier.
    - Each archive has a unique ID and an optional description, which can only be specified during the upload of an archive.
    - Glacier assigns the archive an ID, which is unique in the AWS region in which it is stored.
    - Archive can be uploaded in a single request. While for large archives, Glacier provides a multipart upload API that enables uploading an archive in parts.



    

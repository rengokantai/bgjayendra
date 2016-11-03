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

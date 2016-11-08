## Overview
- Amazon EC2 provides flexible, cost effective and easy-to-use storage options with a unique combination of performance and durability. These include
  - Amazon Elastic Block Store (EBS)
  - Amazon EC2 Instance Store
  - Amazon Simple Storage Service (S3)
- While EBS and Instance store are Block level, Amazon S3 is an Object level storage


## Block Device Mapping
- A block device is a storage device that __moves data in sequences of bytes or bits (blocks)__
and supports random access and generally use buffered I/O for e.g. hard disks, CD-ROM etc

- __Block devices can be physically attached to a computer 
(like an instance store volume) or can be accessed remotely as if it was attached (like an EBS volume)__

- Block device mapping defines the block devices to be attached to an instance, 
which can either be __done while creation of an AMI or when an instance is launched__


- __When a block device is detached from an instance, 
it is unmounted by the operating system and you can no longer access the storage device.__

- __Additional Instance store volumes can be attached only when the instance is launched w
hile EBS volumes can be attached to an running instance.__

- View the block device mapping for an instance, __only the EBS volumes can be seen, not the instance store volumes__.
Instance metadata can be used to query the complete block device mapping.

###Public Data Sets
- Amazon Web Services provides a repository of public data sets that can be seamlessly integrated into AWS cloud-based applications.
- Amazon stores the data sets at no charge to the community and, as with all AWS services, __you pay only for the compute and storage
you use for your own applications.__

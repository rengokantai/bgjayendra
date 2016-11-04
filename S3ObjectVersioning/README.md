##Outline
- Versioning can be used to protect from unintended overwrites and deletions
- Versioning helps to keep multiple variants of an object in the same bucket and can be used to preserve, retrieve, and restore every version of every object stored in your Amazon S3 bucket.
- As Versioning maintains multiple copies of the same objects as whole and you accrue charges for multiple versions
for e.g. for a 1GB file with 5 copies with minor differences would consume 5GB of S3 storage space and __you would be charged for the same.__


- Versioning is not enabled by default and has to be explicitly enabled for each bucket
- __Versioning once enabled, cannot be disabled and can only be suspended__
- __Versioning enabled on a bucket applies to all the objects within the bucket__
- __Permissions are set at the version level.__ Each version has its own object owner; 
an AWS account that creates the object version is the owner. So, you can set different permissions for different versions of the same object.

- Irrespective of the Versioning, each object in the bucket has a version.
  - For Non Versioned bucket, the version ID for each object __is null__
  - For Versioned buckets, __a unique version ID is assigned to each object__
  
- With Versioning, version ID forms a key element to define uniqueness of an object 
within an bucket along with the bucket name and object key

- Object Retrieval
  - For Non Versioned bucket
    - An Object retrieval always return the only object available
  - For Versional bucket
    - An object retrieval returns the Current object.
    - __Non Current object(previous version Object) can be retrieved by specifying the version ID.__


- When an object in a bucket is deleted
  - For Non Versioned bucket
    - An object is permanently deleted and cannot be recovered
  - For Versioned bucket,
    - All versions remain in the bucket and Amazon inserts a delete marker which becomes the Current version
    - A non current versioned object can be retrieved and restored hence protecting against accidental overwrites
    - __If a Object with a specific version ID is deleted, a permanent deletion happens and the object cannot be recovered__







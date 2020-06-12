## Elastic File System (EFS)
[EFS FAQ](https://aws.amazon.com/efs/faq/)

Unlike EBS volumes which can only be used by a single EC2 instance, EFS is a file share that can be used by multiple EC2 instances and it grows/shrinks automatically as files are added and removed.

- Supports the Network File System version 4 (NFSv4) protocol. You only pay for the storage you use.
- Can support thousands of concurrent NFS connections.
- Can scale up to petabytes
- Read After Write Consistency

EFS is more intended for Unix and Linux devices - Amazon does not support Windows EC2 instances using EFS. See FSx for alternative file share options better for Windows.

When attaching EFS to EC2 instances it is necessary to configure bi-directional Security groups on port `2049`

>Note: Use EFS when you need distributed, highly resilient storage for Linux EC2 instances and Linux-based applications

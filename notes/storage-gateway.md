# Storage Gateway
[Storage Gateway FAQ](https://aws.amazon.com/storagegateway/faqs/)

Available as a physical device and virtual machine. Allows a datacenter to attach AWS cloud storage that's scalable and cost effective.

There are three different types of storage that can be attached with a storage gateway:
- File Gateway - NFS or SMB
- Volume Gateway - iSCSI; stored volumes, cached volumes; virtual hard disk drives
- Tape Gateway - Virtual Tape Library, backups

## File Gateway
![file gateway](../img/File%20Gateway.svg)
Files are stored in S3 buckets, accessed through a network file system (NFS) mount point. Ownership, permissions and timestamps are stored in S3 in the metadata associated with the file.

Files stored this way can take advantage of all the benefits of S3 such as versioning, lifecycle management, and cross-region replication.

The most recently-accessed data is cached on the gateway for faster access times.

## Volume Gateway
Provides applications with disk volumes using the iSCSI block protocol.

Changes to these volumes can be asyncronously backed up as point-in-time snapshots and stored in the cloud as `Amazon EBS` snapshots.

Snapshots are incremental backups that capture only changed blocks, snapshots are compressed to minimize storage charges.

### Stored Volumes
![stored volumes](../img/Volume%20Gateway%20-%20Stored%20Volumes.svg)
Primary data is stored locally, streaming to AWS for durable, off-site backups. Everything is stored locally and then asyncronously backed up to `Amazon S3` in the form of Amazon Elastic Block Store `Amazon EBS` snapshots.

### Cache Volumes
![cache volumes](../img/Volume%20Gateway%20-%20Cache%20Volumes.svg)
Only the most frequently-used data is stored locally, which minimizes the need to scale on-premises storage infrastructure.

## Tape Gateway
![tape gateway](../img/Tape%20Gateway.svg)
Offers durable, cost-effective data archival solution in the AWS Cloud. VTL interface lets you use your existing tape-based backup application infrastructure to store data on virtual tape cartridges that you create on your tape gateway.

Archived tapes are stored in S3 Glacier or S3 Glacier Deep Archive

Supported by NetBackup, Backup Exec, Veeam, etc.

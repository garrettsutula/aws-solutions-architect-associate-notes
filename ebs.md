# Elastic Block Store (EBS)
[EBS FAQ](https://aws.amazon.com/ebs/faqs/)

Virtual, cloud-based persistent block storage with different service tiers based on application needs.
## Volume Types
- General Purpose SSD
- Provisioned IOPS SSD
- Throughout Optimized HDD
- Cold HDD
- Magnetic
## Volume Type Comparison
| Volume Type | Description | Use Cases | API Name | Volume Size | Max IOPS per Volume |
| - | - | - | - | - | - |
|General Purpose SSD|Balances price & performance for a wide variety of workloads|Most Workloads|gp2|1 GiB - 16 TiB|16,000|
|Provisioned IOPS SSD|Highest-performance designed for mission-critical apps|Databases|io1|4GiB - 16 TiB|64,000|
|Throughput Optimized HDD|Low-cost HDD for frequently accessed, throughput-intensive workloads|Big Data & Data Warehouses|st1|500 GiB - 16 TiB|500|
|Cold HDD|Lowest-cost HDD designed for less frequently accessed workloads|File Servers|sc1|500 GiB - 16 TiB|250|
|EBS Magnetic|Previous generation HDD|Workloads where data is infrequently accessed|Standard|1 GiB - 1 TiB|40-200|

## Working with EBS Volumes
EBS Volumes are *always* in the same availability zone as the EC2 instance they're attached to.

EBS Volumes can be modified to change the size and type as needed (when the size is changed typically the OS filesystem needs to be expanded).

EBS Snapshots can be used to take a point-in-time backup of a volume which is useful for point-in-time recovery and for making new images based off of snapshots.

Snapshots are *incremental*, only the blocks that have changed are moved to S3.

One practical use of EBS Snapshots is moving EC2 instances between availability zones. By taking a snapshot, creating an image (AMI) and then launching a new EC2 instance in a different availability zone you can effectively "migrate" an instance from one AZ to another.

>Note: It is best practice to stop the EC2 instance prior to taking a snapshot of the root volume when migrating EC2 instances in this way.

AMIs can also be copied to different regions which allows for migrating EC2 instances and their underlying volumes to entirely new regions.

### Encrypted Root Device Volumes & Snapshots
If a root device volume is not already encrypted, you can encrypt it by:
1. Taking a snapshot of the volume
2. Copying the snapshot, select to encrypt it at that time.
3. Create an AMI from the encrypted snapshot.
4. Create a new EC2 instance based on the new AMI.

EC2 now supports creating instances with encrypted root device volumes, so this step would only apply to "legacy" instances.

Snapshots of encrypted volumes are encrypted automatically.

Only can only share unencrypted snapshots. Unencrypted snapshots can be shared with other AWS accounts or be made public.

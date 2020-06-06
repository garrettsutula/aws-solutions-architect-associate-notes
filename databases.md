# Database on AWS

## Relational Database (RDS)
[RDS FAQ](https://aws.amazon.com/rds/faqs/)

`RDS` runs on virtual machines but you cannot log in to these operating systems. Patching is Amazon's responsibility. RDBMS are useful for Online Transaction Processing (OLTP).

`RDS` has two key features:
- Multi-AZ - For disaster recovery, has automatic failover
- Read Replicas - For Performance, horizontal scaling

AWS offers the following relational databases on `RDS`:
- Microsoft SQL Server
- Oracle
- MySQL Server
- PostgreSQL
- Aurora
  - Has a "Serverless" offering
- MariaDB
  
### Backups
Two types of backups:
- Automated Backups
- Snapshot Backups

Anytime backups are restored, the restored version of the database will be a new RDS instance with a new DNS endpoint.

#### Automated Backups
Allow recovery to any point-in-time during the retention period, down to a second. Stores a full daily snapshot plus transaction logs for each day. Backup data is stored in S3, you get free storage space equal to the size of your database. 

Backups are taken within a defined window, during the window storage I/O may be suspended while your data is being backed up and you may experience increase latency.

#### Snapshot Backups
Done manually (user-initiated). Stored even after the original RDS instance is deleted, unlike automated backups.

### Aurora
Aurora is a MySQL and PostgreSQL-compatible relational database engine that combines the simplicity and cost-effectiveness of open-source with the speed and availability of high-end commercial databases.

Up to 5 times better performance than MySQL, 3 times better than PostgreSQL at a lower cost and similar availability.

#### Scalability
- Starts with 10GB, auto-scales in 10GB increments to 64TB
- Compute resources can scale up to 32vCPUs and 244GB of memory
- 2 copies of your data is maintained in each AZ across a minimum of 3 AZs - 6 copies overall

#### Availability
- Write Availability: Up to two copies of data can be lost without losing write availability
- Read Availability: Up to three copies of data can be lost without losing read availability
- Self-healing, data blocks and disks are continuously scanned for errors and repaired automatically

#### Replication
Three types of replicas:
- Aurora - up to 15 replicas
- MySQL - up to 5 replicas
- PostgreSQL - only 1 replica

|Feature|Aurora Replicas|MySQL Replicas|
| - | - | - |
|Number of Replicas|Up to 15|Up to 5|
|Replication Type| Async (milliseconds)| Async (seconds)|
|Performance impact on primary|Low|High|
|Replica Location|In-Region|Cross-region|
|Act as failover target|Yes (no data loss)|Yes (minor data loss)|
|Automated Failover|Yes|No|
|Support for user-defined replication delay|No|Yes|
|Support for different data or schema vs. primary|No|Yes|

#### Backups
- Automated backups are always enabled. 
- You can take snapshot backups.
- Backups do not impact database performance.
- You can share Aurora Snapshots with other AWS accounts.

#### Amazon Aurora Serverless
On-demand auto-scaling configuration for Aurora. Automatically starts up, shuts down and scales based on your application's needs.

Simple, cost-effective option for intermittent, unpredictable or infrequent workloads.

### Encryption at Rest
Encryption at rest is supported for all RDS types and uses the AWS Key Management Service (KMS). Once the RDS instance is encrypted, the data, automated backups, read replicas and snapshots are also encrypted.

### Multi-AZ
Replicates to a secondary database instance with automatic failover at the load-balancer level. DNS is updated to point to the secondary RDS instance's IP address and normal operations resume. You can force a failover by rebooting with that option checked.

Available for:
- SQL Server
- Oracle
- MySQL Server
- PostgreSQL
- MariaDB

Aurora is fault tolerant by design and doesn't have a concept of Multi-AZ for disaster recovery.

### Read Replica
Replicates to one or more read replicas asyncronously, each replica can be read from but not written to and replicas can replicate to additional replicas if needed.

Up to 5 read replica copies of any database. Read replicas can have read replicas but latency will increase (possibly suitable for reporting that isn't near-realtime). You can have a read replican in a second region.

Read replicas can be promoted to be their own databases. This breaks the replication.

Good for read-heavy database workloads where a cache is not useful.

## Non-Relational Databases
Amazon has a couple different non-relational database options:
- DynamoDB

### DynamoDB
[DynamoDB FAQ](https://aws.amazon.com/dynamodb/faqs/)

Fast & flexible NoSQL database service, consistent single-digit millisecond latency at any scale. Fully managed database, supports both `document` and `key-value` data models.

- Stored on SSD storage
- Spread across 3 geographically distinct data centers
- Supports both Eventually Consistent Reads (Default) and Strongly Consistent Reads


## Amazon Redshift
`Amazon RedShift`, Data Warehouse solution for Online Analytics Processing (OLAP) and Business Intelligence.

Fully managed, petabyte-scale but can start small for just $0.25/hr with no commitments or upfront costs. At petabyte scale, cost ~$1000/TB/yr which is less than a tenth of most other solutions.

Redshift can be configured in two ways:
- Single Node
- Multi-Node
  - Leader Node - manages connections and queries from clients
  - Compute Node - Stores data and performs queries/computations. Up to 128 compute nodes.

Uses column data stores which can be compressed much more than row-based data because similar data is stored sequentially on disk. When loading data into a table, Redshift automatically samples data and selects the most appropriate compression scheme.

Redshift doesn't required indexes or materialized views, uses less space than traditional RDBs. Redshift automatically distributes data and query load across all nodes. It's straightforward to add nodes which supports horizontal scaling needs to maintain fast querys.

Redshift can asynchronously replicate your snapshots to S3 in another region for disaster recovery.

### Pricing
- Based on Compute Node Hours (total # of hours across all compute nodes in a billing cycle)
- Backup
- Data Transfer (only within a VPC, not outside it)

### Backups
- Enabled by default with 1 day retention period.
- Maximum retention period is 35 days.
- Redshift always attempts to maintain at least three copies of your data (original and replica on compute nodes, backup in Amazon S3).

### Security
- Transport-level security (SSL/TLS)
- Encrypted at Rest
- By default RedShift takes care of key management
  - Can manage your own keys through Hardware Security Module (HSM)

### Availability
- Currently only available in 1 AZ
- Can restore snapshots to new AZs in the event of an outage

## In-Memory Caching
ElastiCache is cloud-based in-memory caching, used to increase database and web application performance. 

Supports two open-source in-memory caching engines:
- Redis
- Memcached

|Requirement|Memcached|Redis|
| - | - | - |
|Simple Cache to Offload DB|Yes|Yes|
|Ability to scale horizontally|Yes|No|
|Multi-threaded Performance|Yes|No|
|Advanced Data Types|No|Yes|
|Ranking/Sorting Data Sets|No|Yes|
|Pub/Sub Capabilities|No|Yes|
|Persistence|No|Yes|
|Multi-AZ|No|Yes|
|Backup & Restore Capabilities|No|Yes|

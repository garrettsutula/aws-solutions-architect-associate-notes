# Amazon Elastic Cloud Compute (EC2)
[EC2 FAQ](https://aws.amazon.com/ec2/faqs/)

Ability to run and scale virtual machines on-demand. Reduces the required time to obtain and boot new servers, scale up or down according to demand. The Amazon Nitro hypervisor is used to run VMs, formerly the Xen Hypervisor was used.

## Pricing Models
Four pricing models:
1. **On-Demand** - Fixed rate by the hour with no commitment. Great for developers.
2. **Reserved** - Provides a capacity reservation, significant discount on hourly charge. 1 year or 3 year terms.
3. **Spot** - Amazon has excess capacity, drops prices of EC2 instances to bid for cheaper EC2 instances.
4. **Dedicated Hosts** - Physical EC2 servers dedicated for your use, can help reduce cost by using existing server-bound licensing (e.g. Oracle)

### On-Demand Pricing
Good for:
- Developers
- Users that want no up-front cost or long-term commitment
- Applications with short term, spiky or unpredictable workloads that can't be interrupted

### Reserved Pricing
Good for:
- Applications with steady state or predictable usage
- Applications that require reserved capacity
- Users able to make up-front payments to reduce total costs further over time.

#### Reserved Pricing Types
1. **Standard Reserved Instances** - up to 75% off on-demand instances, discount increases with longer terms and larger up-front payment
2. **Convertible Reserved Instances**- up to 54% off, on-demand capability to change the attributes of the RI as long as the exchange results in equal or greater value (cost).
3. **Scheduled Reserved Instances** - reserve specific time windows, allows you to match capacity with a predictable schedule whether it's a part of a day, week or month.

Instance type, scope, tenancy and hardware platform determine the cost of a reserved instance.

### Spot Pricing
Good for: 
- Applications that are only feasible at very low compute prices
- Applications that have flexible start and end times
- Users with urgent computing needs for large amounts of additional capacity

Amazon will terminate instances if demand for higher-priced tiers increases. If Amazon terminates, you are not charged for a partial hour of usage but if you terminate you are charged.

### Dedicated Host Pricing
Good for:
- Licensing that doesn't support multi-tenancy or cloud deployments.
- Any requirement (e.g. regulatory) that may not support multi-tenant virtualization.

Can be purchased On-Demand or Reserved for a discount (70% vs 75%).

## EC2 Instance Types
**Not typically tested in Solutions Architect exam.**
| Family | Speciality | Use Case |
|- | -| -|
|F1|Field Programmable Gate Array|Genomics Research, Financial Analytics, Real-time video processing, Big Data, etc.
|I3|High-Speed Storage|NoSQL DBs, Data Warehousing, etc.|
|G3|Graphics-Intenstive|Video Encoding, 3D Application Streaming|
|H1|High Disk Throughput|Map-Reduce, Distributed File Systems (HDFS, MapR-FS)|
|T3|Lowest Cost General Purpose|Web Servers, Small DBs|
|D2|Dense Storage|Fileservers/Data Warehousing/Hadoop|
|R5|Memory Optimized|Memory-intensitve Applications/Databases|
|M5|General Purpose|Application Servers|
|C5|Compute Optimized|CPU Intensitve Applications/Databases|
|P3|General Purpose GPU|Machine Learning, Bitcoin Mining, etc.|
|X1|Memory Optimized|SAP HANA, Apache Spark, etc.|
|Z1D|High Compute Capacity and High Memory|Electronic Design Automation, Certain RDBMS with high per-code licensing costs|
|A1| ARM-based workloads|Scale-out workloads such as webservers|
|U-6tb1|Bare Metal|Bare metal capabilities that eliminate virtualization overhead|

## Working with EC2
- Stop and Termination Protection is turned off by default
- Default action is to delete the root volume on termination, additional EBS volumes are not deleted by default
- Root and Additional Volumes can be Encrypted as part of provisioning, can use third party tools or Amazon's encryption offerings.
- EBS Volumes are **always in the same availability zone** (datacenter) as the EC2 instance they're attached to.

### Security Groups
Security Groups are basically virtual firewalls that control how EC2 instance interfaces are exposed to the internet. Security groups can be assigned to zero or more EC2 instances.

Inbound/Outbound rule changes take place **immediately**. Rules are *stateful* and whitelist only, connections that are allowed inbond are also allowed outbound. VPC Network Access Control Lists (NACLs) are *stateless* and allow control over one-way firewall rules and blacklisting.

Security Groups block traffic by default following the least-privilege principle (whitelisting).

### AMIs (images)
The AMI used for an EC2 instance can be selected by:
- Region
- Operating System
- Architecture (x86/ARM, 32-bit or 64-bit)
- Launch Permissions
- Storage for the Root Device (Root Device Volume)
  - Instance Store (Ephemeral Storage) - created from a templated store in Amazon S3
  - EBS Backed Volumes - created from an EBS snapshot

Images can be created from EBS snapshots. Virtualization type is typically Hardware-assisted virtualization (HVM) for maximum compatibility.

Instances using either type of storage can be rebooted. By default, both types will delete the root volume on termination but with EBS volumes you can choose to not delete the volume.
#### Instance Store Volumes
Sometimes called Ephemeral Volumes. Instance store volumes **cannot** be stopped. If the underlying host fails, you lose your data.

#### EBS Volumes
Can be stopped, you will not lose data if the EC2 instance is stopped.

EBS volumes can also be detached while an instance is running, but it will take some time.

### ENI vs EN vs EFA
- **ENI** - *Elastic Network Interface* - basically a virtual network card
- **EN** - *Enhanced Networking* - Uses single root I/O virtualization (SR-IOV) to provide high-performance networking capabilities on supported instance types.
- **EFA** - *Elastic Fabric Adapter* - A network device that you can attach to your Amazon EC2 instance to accelerate High Performance Computing (HPC) and machine learning applications.

#### Elastic Network Interface
Attached automatically to an EC2 instance. Does the following: 
- Binds a primary private IPv4 address from the IPv4 address range of your VPC. 
- Optionally, binds one or more secondary private IPv4 addresses from the IPv4 address range of your VPC
- Binds one Elastic IP address (IPv4) per private IPv4 address
- Binds One public IPv4 address
- Binds One or more IPv4 addresses
- Configured with one or more security groups
- A MAC address
- A source/destination check flag
- A description of the interface

Multiple ENIs are used for:
- Creating a management network
- Using network and security appliances in your VPC
- Creating dual-homed instances with workloads/roles on distinct subnets (e.g. production subnet vs. database subnet)
- Create low-budget, high-availability solutions

Sometimes ENIs are not capable of the network throughput needed for more demanding applications, that's what the other network interfaces are for.

>Note: In any exam scenario question related to basic networking, network segmentation and you want to do this at a low cost ENIs are the answer.

#### Enhanced Networking
Uses single root I/O virtualization (SR-IOV) to provide high-performance networking capabilities on supported instance types. SR-IOV is a method of device virtualization that provides higher I/O performance and lower CPU utilization when compared to traditional virtualized network interfaces.

EN provides higher bandwidth, higher packets per second (PPS) performance, and consistently lower inter-instance latencies. **There is no additional charge for using enhanced networking**

Use this when good network performance really matters.

There are two options for EN:
- Elastic Network Adapter (ENA) - supports network speeds of up to **100 Gbps** for supported instance types.
- Intel 82599 Virtual Function (VF) - supports network speeds of up to **10 Gbps** for supported instance types. This is typically used on older instances.

>Note: In any exam scenario question, you probably want to choose ENA over VF if given the option. ENA is also the answer for when you need speeds between 10Gbps & 100Gbps

#### Elastic Fabric Adapter
A network device that you can attach to your Amazon EC2 instance to accelerate High Performance Computing (HPC) and machine learning applications.

EFA provides lower and more consistent latency, higher throughput than the TCP transport traditionally used in cloud-based HPC systems.

EFA can use OS-bypass. OS-bypass enables HPC and machine learning applications to bypass the operating system kernel and to communicate directly with the EFA device. It makes it a **lot** faster with a **lot** lower latency. Not supported on Windows currently, only Linux.

>Note: In any exam scenario question that mentions HPC or ML that asks what network adapter you want, choose EFA.

### Bootstrap Scripts
Bootstrap scripts are run on instance first boots, can be used to automate software installation, configuration and updates.

### Instance Metadata
From within your EC2 instance you can retrieve instance user and metadata via HTTP requests.
- User Data (startup script) - `http://169.254.169.254/latest/user-data/`
- Metadata (instance information) - `http://169.254.169.254/latest/meta-data/`

This is useful for scripting. For example, you can retrieve the public IPv4 address of an EC2 instance with `curl http://169.254.169.254/latest/meta-data/public-ipv4`

### EC2 Placement Groups
The way to place EC2 instances, there are three types:
1. Clustered
2. Spread
3. Partitioned

Limitions on placement groups:
- Names for placement groups must be unique within your AWS account. 
- Only certain types of instances can be launched within a placement group. 
- You cannot merge placement groups.
- Instances can be moved or removed from placement groups, the instance must be in the "stopped" state.

#### Clustered Placement Groups
Grouping of EC2 instances within a single availability zone. 

When to use: applications that need low network latency, high network throughput or both. Improved performance.

Only certain instances can be launched into a Clustered Placement Group. A clustered placement cannot span multiple availability zones. AWS recommends homonogenous instances within Clustered Placement Groups.

If you get capcity errors, try stopping and restarting all instances to get them running on dedicated hardware with sufficient capacity.

#### Spread Placement Groups
Opposite to a Clustered Placement Group, a group of instances that are each placed on separate hardware. Improved durability.

When to use: applications that have a small number of critical EC2 instances that should be kept separate from each other.

You can have up to seven instances per availability zone per placement group.

#### Partitioned Placement Groups
Divides groups of instances into logical partitions that do not share hardware. 

When to use: applications that use multiple EC2 instances to function and cluster across partitions

Multiple instances are grouped together, then spread across hardware.

>TODO: study building VPCs from memory including subnets, routes, nat gateways
>TODO: VPC Diagrams

# Virtual Private Cloud
[VPC FAQ](https://aws.amazon.com/vpc/faqs/)

Amazon VPC lets you provision a logically-isolated section of AWS where you can launch AWS resources in a virual network you define and control.

Some types of vuln scans are ok to do without contacting AWS, others need approval [see: vuln testing](https://aws.amazon.com/security/penetration-testing/)

You have complete control over your virtual networking environment, including IP address range, creation of subnets and configuration of route tables and network gateways.

One example: you can easily create a "public" subnet for webservers with internet access and a "private" subnet for database or application servers with no internet access.

Multiple layers of security can be implemented to protect your AWS resources including:
- Security Groups (AWS Resources such as EC2, RDS)
- Network Access Control Lists (VPC)

Additionally, you can create a Hardware Virtual Private Network (VPN) connection between your corporate datacenter and your VPC to use the AWS cloud as an extension of your datacenter.

>Note: The largest IP range supported in AWS is a `/16`, the smallest is a `/28` use [cidr.xyz](cidr.xyz) to visualize ip ranges.

VPC Components:
- Internet Gateways (or Virtual Private Gateways)
- Route Tables
- Network Access Control Lists
- Subnets - subnets cannot be spread across availability zones
- Security Groups

![vpc-overview]()

What can be done with a VPC?
- Launch instances into a chosen subnet
- Assign custom IP ranges for each subnet
- Configure route tables between subnets
- Create internet gatteway and attach it to our VPC
- Implement much better security control with NACLs
- Instance security groups
- Subnet network access control lists

## Default VPC vs Custom VPC
  - User-friendly, good for development
  - All subnets in default VPC have a route out to the internet
  - Each EC2 instance has both a public and a private IP address


## VPC Peering
- Allows you to connect one VPC to another via a direct network route using private IP addresses.
- Instances behave like they're on the same private network.
- You can peer VPCs with other AWS accounts as well as other VPCs in the same account.
- You can peer between regions
- VPC peering is always direct between two VPCs, e.g. `A <-> B`, with a `A <-> B <-> C` configuration `A` & `C` can only talk to `B`, but `B` can talk to `A` & `C`

## Creating a VPC
Maximum 5 VPCs per Region
Maximum 5 IPv4 CIDR blocks per VPC
Maximum 1 IPv6 CIDR blocks per VPC

When creating a new VPC, default a route table, Network ACL and Security Group will be set up automatically, like this:
![new-vpc](img/new-vpc.png)

No subnets or internet gateways are created by default.

Availability Zones (e.g. `US-East-1`) are randomized between accounts and can be different in another AWS account.

### Subnets
Maximum `200` subnets per VPC

One or more subnets must be created in the VPC. When subnets are created, five ip addresses will be reserved for use by AWS. If the subnet is `10.0.0.0/24`:

- `10.0.0.0`: Network address.
- `10.0.0.1`: Reserved by AWS for the VPC router.
- `10.0.0.2`: Reserved by AWS. The IP address of the DNS server is the base of the VPC network range plus two. For VPCs with multiple CIDR blocks, the IP address of the DNS server is located in the primary CIDR. We also reserve the base of each subnet range plus two for all CIDR blocks in the VPC. For more information, see Amazon DNS server.
- `10.0.0.3`: Reserved by AWS for future use.
- `10.0.0.255`: Network broadcast address. We do not support broadcast in a VPC, therefore we reserve this address.

### Internet Gateways
Must be attached to a VPC to allow ingress from the internet, only one internet gateway can be attached per VPC.

After configuring Subnets and Internet Gateways for one public and one private subnet a VPC would look like this:

### NAT Gateways & NAT Instances
NAT Gateways spread across multiple availability zones and is a highly-available gateway. 

NAT instances are individual EC2 instances that provide Network Address Translation, legacy but still in the exam.

#### NAT Instances
[NAT Instances Documentation](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_NAT_Instance.html)

Created from a Community Image, add to a security group with internet acces (e.g. WebDMZ) then Disable Source/Destination checks in Instance Networking settings. Finally, add a route to the main route table like `0.0.0.0/0` pointed to the NAT instance and the instances in the private subnet should have internet access.

Downsides:
- The amount of traffic the NAT instance can support depends on the instance size. If you are bottlenecking, increase the instance size.
- You can create high availability using Autoscaling Groups, multiple subnets in different AZs, and a script to automate failover.

#### NAT Gateway
NAT Gateways are simply assigned to a subnet with internet access, assiged and elastic IP then created. After creation, add a route to the main route table pointed to the NAT gateway (similar to the configration for NAT instances).

- Redundant inside Availability Zone
- Preferred by the enterprise
- Start at 5Gbps and scales currently to 45GBps
- No need to patch
- Not associated with Security Groups
- Automatically assigned a public IP address

If you have resources in multiple availability zones and they share one NAT gateway, all resources will lose internet access if the NAT gateway's availability zone is down.

To create an AZ-independent architecture, create a NAT gateway in each AZ and configure routing to ensure the resources in each AZ use the NAT gateway in the same AZ.

### Network ACLs
Can be created to establish inbound and outbound traffic rules that apply to one or more subnets. 
- The default NACL created with a VPC allows all traffic. 
- New NACLs default to deny all connections inbound & outbound.
- Each subnet in your VPC must be associated with a NACL. If you don't explicitly associate a subnet, it uses the default.
- Block IP addresses using network ACLs not security groups
- Default limit of `20` inbound and outbound rules per NACL, maximum is `40`.
- NACLs are stateless, responses to inbound traffic are subject to outbound rules (and vice versa).

Inbound rules should only include the ports/protocols that are used by the instances in the subnet(s). Outbound rules should include the ports used by the instances in the subnet(s) and [ephemeral ports](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html#nacl-ephemeral-ports).

Rules are evaluated in ascending order e.g. if Rule 100 and Rule 300 overlap Rule 100 wins whether it allows or denys the connection.

Network ACLs are *always* evaluated before security groups.

### VPC Flow Logs
Enables you to capture information about the IP traffic going to and from network interfaces in your VPC. Flow log data is stored using Amazon CloudWatch.

Flow Logs can be created at three levels:
- VPC
- Subnet
- Network Interface

Flow Log Tips:
- You cannot enable flow logs for a VPC that is peered but not in your account.
- You can tag flow logs
- After you create a flow log, you cannot change its configuration. For example, you can't associate a different IAM role with the flow log.
- Not all IP traffic is monitored including:
  - Traffic generated by instances when they contact Amazon DNS
  - Traffic generated by a windows instance for Amazon Windows license activation.
  - Traffic to and from the instance metadata IP address (`169.254.169.254`)
  - DHCP Traffic
  - Traffice to the reserved IP address for the default VPC router

### Bastion Hosts
A special purpose computer on a network designed to withstand attacks typically by disabling all services except the ones needed for its single purpose along with other hardening methods.

- A NAT Gateway or NAT instance is used to provide outbound internet access to EC2 instances in private subnets.
- A bastion is used to securely administer EC2 instances using SSH or RDP.
- You cannot use a NAT gateway as a bastion host.

### Direct Connect
Direct Connect is a cloud service solution that lets you establish a dedicated network connection from your premises to AWS. Using AWS Direct Connect, you can establish private connectivity between a physical site & AWS which can reduce network costs, increase bandwidth throughput, and provide a more consistent network experience than internet-based connections.

![direct-connect](img/direct-connect.png)

- Direct Connect directly connects your datacenter to AWS
- Useful for high throughput workloads
- Or if you need a stable/reliable secure connection

Steps to set up Direct Connect ([How-to Video](https://www.youtube.com/watch?v=dhpTTT6V1So&feature=youtu.be)):
1. Create a virtual interface in the Direct Connect console. This is a **Public Virtual Interface**
2. Go to the VPC Console and then to VPN connections. Create a Customer Gateway
3. In the same place, create a Virtual Private Gateway
4. Attach the Virtual Private Gateway to the desired VPC
5. Select VPN Connections and create a new VPN Connection
6. Select the Virtual Private Gateway and the Customer Gateway
7. Once the VPN is available, set up the VPN on the customer gateway or firewall.

### Global Accelerator
Service used to create accelerators to improve availability and performance of your applications for local and global users.

Global Accelerator directs traffic to optimal endpoints over the AWS global network. This improves the experience for a global audience.

By default, Global Accelerator is provisioned with `two` static IP addresses associated to your accelerator. Alternative, you can bring your own.

Global Accelerator Components:
- Static IP Addresses
  - Two assigned on creation
- Accelerator -  
  - Directs traffic to optimal endpoints over the global AWS network
  - Includes one or more listener
- DNS Name - 
  - Assigned on creation
  - Can be used instead of IP addresses
- Network Zone - services the static IP addresses for your accelerator from a unique IP subnet. 
  - Similar to an AZ.
  - Each IP address points to a different Network Zone.
- Listener
  - Processes inbound connections based on configured port/port ranges
  - Supports TCP and UDP
  - Each listener has one or more endpoint groups associated with it, traffic is forwarded to endpoints in one of the groups.
  - Endpoint groups are associated by specifying the Regions you want to distribute traffic to.
- Endpoint Group
  - Each associated with a specific AWS Region
  - Include one or more endpoints in the region (EC2 instances, ELB, etc.)
  - Has a traffic dial to increase or decrease traffic regardless of default behavior
  - Traffic dial makes it easy to performance test and do blue/green deployment testing.
- Endpoint
  - Include:
    - Network Load Balancers
    - Application Load Balancers
    - EC2 Instances
    - Elastic IP Addresses
  - Can be internal or internet-facing
  - Can set weights to specify the proportion of traffic to route to each endpoint.

### VPC Endpoints
Allows you to privately connect your VPC to supported AWS services and VPC endpoint services powered by PrivateLink without requiring an Internet Gateway, NAT device, VPN connection or AWS Direct Connect connection.

Instances in your VPC do not require public IP addresses to communicate with resources in the service.

Traffic between your VPC and the other service doesn't leave the Amazon network.

VPC Endpoints are virtual devices. They are horizontally scaled, redundant and highly available VPC components that allow communication between instances in your VPC and services without imposing availability risks or bandwidth constraints on your network traffic.

Two types:
- Interface Endpoints
- Gateway Endpoints

#### Interface Endpoints
Is an elastic network interface (ENI) with a private IP address that serves as an entry point for trafic destined to a supported service. 

The following services are supported:
- Amazon API Gateway
- CloudFormation
- CloudWatch
- CloudWatch Events
- CloudWatch Logs
- CodeBuild
- Config
- EC2 API
- Elastic Load Balancing API
- AWS Key Management Service
- Kinesis Data Streams
- Sagemaker and Sagemaker Runtime
- SageMaker Notebook Instance
- AWS Secrets Manager
- AWS Security Token Service
- AWS Service Catalog
- Amazon SNS
- Amazon SQS
- AWS Systems Manager
- Endpoint services in other AWS accounts
- Supported AWS Marketplace partner services

#### Gateway Endpoints
Is an elastic network interface (ENI) with a private IP address that serves as an entry point for trafic destined to a supported service. 

- S3
- DynamoDB

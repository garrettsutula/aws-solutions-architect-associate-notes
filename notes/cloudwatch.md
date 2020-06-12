# CloudWatch
[CloudWatch FAQ](https://aws.amazon.com/cloudwatch/faqs/)

Cloudwatch is a monitoring sevice to monitor your AWS resources as well as the applications you run on AWS. Cloudwatch is all about performance monitoring and metrics.

Cloudwatch can monitor things like:
- Storage & Content Delivery
  - EBS Volumes
  - Storage Gateways
  - Cloudfront
- Compute
  - EC2 Instances
  - Autoscaling Groups
  - Elastic Load Balancers
  - Route53 Health Checks

## Cloudwatch & EC2
CloudWatch with EC2 will monitor metrics every 5 minutes by default (detailed monitoring @ 1 minute intervals can be enabled) and can monitor:
- CPU
- Network
- Disk
- Status Check
  - Hypervisor Status
  - EC2 Instance Status

Alarms can be created which trigger notifications based on performance criteria.

>Note: Exam questions like to try to confuse you between CloudWatch and CloudTrail. Cloudwatch is for performance monitoring & metrics, CloudTrail is for SIEM and auditing.

## Dashboards
Build dashboards, can be regional or global.

## Logs
You can send logs to CloudWatch for analytics and log-based metrics.

## Events
Near-realtime steam of events that describe changes to AWS resources.

# Auto-Scaling (EC2 & AWS)
[EC2 Auto-Scaling User Guide](https://docs.aws.amazon.com/autoscaling/ec2/userguide/what-is-amazon-ec2-auto-scaling.html)

EC2 Autoscaling has 3 components:
1. Groups - Logical component. A webserver group, application group, database group, etc.
2. Configuration Templates - Groups use a launch template or launch configuration for their EC2 instances. You can specify information such as the AMI, instance type, key pair, security groups, block device, etc.
3. Scaling Options - Defines how to scale groups. Can be configured on a schedule or occurence of specified conditions (dynamic scaling).

## Scaling Options
1. Maintain Current Instance Levels at All Times
2. Scale Manually
3. Scale Based On a Schedule
4. Scale Based On Demand
5. Use Predictive Scaling

### Maintain Current Instance Levels at All Times
Configured health check makes sure instances are healthy, terminates unhealth instances and launches replacements.

### Scale Manually
Simple configuration of maximum, minimum or desired capacity of auto-scaling group. AWS creates or terminates instances to match this configuration

### Scale Based On a Schedule
Scaling actions are performed automatically as a function of time and date.

Useful when you know exactly when you'll want to increase or decrease the instances in your auto-scaling group.

### Scale Based On Demand
Most popular type of scaling. Allows you to define policies that control the scaling process. E.g. CPU utilization.

### Predictive Scaling
Using AWS Auto-Scaling (different than EC2 Auto-Scaling) to scale resources across multiple services. Combines predictive and dynamtic scaling (proactive and reactive) to scale your Amazon EC2 capacity faster.

[AWS Autoscaling FAQ](https://aws.amazon.com/autoscaling/faqs/)

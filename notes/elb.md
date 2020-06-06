# Elastic Load Balancer (ELB)
[ELB FAQ](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/what-is-load-balancing.html)

Never have a pre-defined IPv4 addrress, you resolve to them with a DNS name.

You need at least two public subnets to create a internet-facing load balancer.

`504 - Gateway Timeout` means the application behind the load balancer is not responding within the timeout period.

Instances are monitored by ELB and reported as `InService` or `OutOfService` based on health checks.

## Types of ELBs
There are three types of load balancers in AWS:
1. Application Load Balancers
2. Network Load Balancers
3. Classic Load Balancers

### Application Load Balancers
Best suited for HTTP/HTTPS, operate at Layer 7 and are application-aware. Intelligent, can handle advanced request routing, sending specified requests to specific web servers.


### Network Load Balancers
Best suited for TCP traffic where extreme performance is required. Operates at L4, can handle millions of requests per second at very low latency.

### Classic Load Balancers
Legacy Elastic Load Balancers. Operates at Layer 4 with some Layer 7 features such as `X-Forwarded-For` and sticky sessions. HTTP/HTTPS or strict Layer 4 mode for TCP.

A bit cheaper than ALBs, good for very basic load balancing.

## Load Balancer Features

### Sticky Sessions
Classic - allows you to bind a user's session to a specific EC2 instance. Good for stateful sessions.

Application - supported as well but requests are sent at the Target Group, not instance, level.

### Cross-Zone Load Balancing
When provisioning a load balancer you pick the availability zones where it'll run instances. If your targets are unevenly distributed in those availability zones, cross-zone load balancing will ensure that the load is still split evenly across them.

### Path Patterns
Load balancer forwards requests based on the URL path, and route requests to different target groups. Useful for microservices architecture.



# Route 53
[Route 53 FAQ](https://aws.amazon.com/route53/faqs/)

Amazon's DNS service. Up to 50 domain names can be managed in Route53 by default but this number can be increase by contacting Amazon support.

>Note: Elastic Load Balancers do not have IPv4 addresses, you always resolve to them using a DNS name.

## About DNS
- Top-level domains are the last word of the domain name, e.g. `.com`, `.edu`, `.gov`.
- Top Level Domains are controled by the Internet Assigned Numbers Authority (IANA).
- Domain registrars share the responsibility of domain name assignment, registered InternNIC a service of ICANN.
- Every DNS address begins with a Start of Authority record (SOA) which includes
  - Name of the server that supplied the data for the zone
  - Administrator of the zone
  - Current version of the data file
  - Default number of seconds for the TTL on resource records
- Name Server Records (NS Records) used by the Top Level Domain servers to direct traffic to the Content DNS server which contains the authoritative DNS records for a given domain
- "A" records are the fundamental type of DNS record. The "A" in A record stands for "Address". The A record is used to translate the name of a domain to an ip address.
- "AAAA" records are the same as "A" records but for ipv6.
- A Canonical Name (CName) record can be used to resolve one domain name to another.
- Alias records are used to map resource record sets in your hosted zone to Elastic Load Balancers, Cloudfront distributions or S3 buckets that are configured as websites.
  - Alias records can redirect to the root of a domain name (also known as a zone apex record), e.g. `yahoo.com` and CNAME records cannot.
  - Alias records have Route53-specific functionality for routing traffic to CloudFront distributions and Amazon S3 buckets.
- MX Records are used for e-mail
- PTR Records are the opposite, a way to look up a name using an ip address

## Routing Policies
- You can buy domain names directly with AWS
- It can take up to 3 days to register depending on circumstances.

There are different types of routing policies:
- Simple Routing
- Weighted Routing
- Latency-based Routing
- Failover Routing
- Geolocation Routing
- Geoproximity Routing
- Multivalue Answer Routing

### Simple Routing
Simple routing will return all values to a user in a random order, first ip that resolves is used.

### Weighted Routing
Allows you to split traffic based on different weights assigned.

### Latency-based Routing
Allows you to route your traffic based on the lowst network latency for the user (i.e. which region will give them the fastest response time).

### Failover Routing
Used when you want to create an active/passive configuration. Route 53 will monitor the health of your primary site using a health check.

### Geolocation Routing
Allows you to route your traffic based on the geographic location of your users (i.e. the location from which DNS queries originate).

Why use instead of latency? More control for regulatory reasons or otherwise. For example, you may want to direct all queries from Europe to a specific destination.

### Geoproximity Routing
Allows Amazon Route 53 to route traffic to your resources based on the geographic location of your users **AND** your resources.

You can optionally route more or less traffic to a given resource by specifying a value, known as a *bias*.

>Note: to use geoproximity routing you must use Route53 **Traffic Flow**, a workflow designer to create a **Traffic Policy**.

### Multivalue Answer Routing
The same as simple routing with one key difference: allows for health-checks on each record set.

## Health Checks
Route 53 has an option to associated health checks to DNS record sets. If a record set fails a health check, it will be removed from Route53 until it passes the health check.

You can set up SNS notifications to alert you if a health check has failed.



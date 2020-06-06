# Web Application Firewall (WAF)
[WAF FAQ](https://aws.amazon.com/waf/faq/)

AWS WAF is a web application firewall that lets you monitor the HTTP and HTTPS requests that are forwarded to Amazon CloudFront, an Application Load Balancer or API Gateway. OSI Layer 7.

AWS WAF also lets you control access to your content.

You can configure conditions such as:
- What IP addresses/ranges are allowed to make a request
- Countries that are allowed to make requests
- What query string params, headers, etc. need to be passed for the request to be allowed
- Strings that appear in requests via regex matching
- Length of requests
- SQL Injection detection
- XSS detection

The request must match the WAF rules or an HTTP `403` Status Code will be returned.

WAF can be configured to whitelist, blacklist, or simply enumerate requests based on configuration.

>Note: Besides AWS WAF you can use Network ACLs to block malicious traffic.

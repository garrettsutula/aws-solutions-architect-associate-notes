# API Gateway (ACG)
[API Gateway FAQ](https://aws.amazon.com/api-gateway/faqs/)

Amazon's API Gateway offering. Publish, maintain, monitor and secure APIs.

Often used in front of Lambda functions, but can be put in front of any web application (e.g. EC2 insances hosting REST APIs, DynamoDB)

Steps to configure:
- Define an API
- Define Resources, nested Resources (URL paths)
  - For each Resource:
    - Select supported HTTP methods
    - Set Security
    - Choose Internal Target
    - Set request & response transformations

Supports AWS Certificate Manager (free SSL/TLS certs).

Supports caching to increase performance, reduce number of calls made to endpoints. When caching is enabled you set a TTL.

Supports throttling to mitigate attacks.

CORS support and configuration is managed at the gateway level.

Can log to CloudWatch.

Low cost, scales automatically.

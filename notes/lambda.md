# Lambda
[Lambda FAQ](https://aws.amazon.com/lambda/faqs/)

Serverless compute service where you can upload your code and create Lambda functions. Lambda takes care of provisioning and managing the servers that are used to run the code. You don't have to worry about patching, scaling, etc.

Execution can be multi-threaded.

Commonly used for:
- Event-driven applications

![lambda-event](../img/Event-driven%20Application.svg)
- HTTP request handlers

![lambda-request](../img/Request-driven%20Application.svg)

Lambda execution is instanced and isolated for each event/request. Lambda functions can trigger other lambda functions to orchestrate functionality.

API Gateway > Lambda > S3/DynamoDB/Aurora Serverless/etc.

Languages Supported:
- Node.js
- Java
- Python
- C#
- Go
- PowerShell

Billed based on:
- Number of requests - first 1M are free
- Duration - rounded up to the nearest 100ms, charged based on memory allocation (GB-seconds used).

AWS X-Ray is used to debug.

Lambda can do things globally, e.g. back-up S3 buckets to other S3 buckets.

Triggers:
- API Gateway
- AWS IoT
- Alexa
- Application Load Balancer
- CloudFront
- CloudWatch Events & LOgs
- CodeCommit
- Cognito Sync
- DynamoDB
- Kinesis
- S3
- SNS
- SQS
- Partner event sources 

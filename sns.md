# Simple Notification Service (SNS)
[SNS FAQ](https://aws.amazon.com/sns/faqs/)

Amazon Simple Notification Service is a web service that makes it easy to set up and send notifications from AWS. Highly-scalable, flexible and cost-effective.

Supports different types of endpoints:
- E-Mail
- SMS
- SQS
- HTTP
- Mobile Push
  - Apple
  - Google
  - Fire OS
  - Baidu Cloud Push (China)

Multiple recipients can be grouped into **Topics**. Each topic can deliver to multiple endpoint types.

All messages are stored redundantly across multiple availability zones.

Benefits:
- Instantaneous delivery (push)
- Simple APIs and integrations
- Flexibile delivery, multiple protocols
- Inexpensive, pay-as-you-go model
- Simple confguration via AWS console

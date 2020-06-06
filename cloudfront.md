# CloudFront
[CloudFront FAQ](https://aws.amazon.com/cloudfront/faqs/)

CloudFront is a Content-Delivery Network (CDN) that uses AWS Edge Locations.

Edge Location - the location where content will be cached, this is separate from regions/availabiliy zones

Origin - The origin of all the files the CDN will distribute. This can be a S3 bucket, EC2 instance, Elastic Load Balancer, or Route53.

Distribution - The name given to the CDN which is a collection of edge locations.

Objects are cached with a TTL on the edge locations. Edge locations are not just read-only - i.e. Transfer Acceleration. You can invalidate the cache, you will be charged for it.

Two types of distribution:
- Web Distribution
- RTMP - Real Time Messaging Protocol. Used for media streaming.

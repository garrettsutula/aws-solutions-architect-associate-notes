# Kinesis
[Kinesis Documentation](https://docs.aws.amazon.com/kinesis/index.html)

Handles streaming data, which is generated continuously by many data sources simultaneously.

Kinesis is a hell of a lot like Apache Kafka.

Examples:
- Stock prices
- Game data
- Social network data
- Geospatial data
- IoT sensor data

Kinesis makes it easy to load and analyze steaming data, build applications.

Three types of Kinesis:
- Kinesis Streams
- Kinesis Firehose
- Kinesis Analytics

## Kinesis Streams
Producers stream data to Kinesis. Kinesis by default stores data for 24 hours to 7 days based on configuration. 

Data is sharded by type and EC2 instances (known as Consumers) analyze the data then store it in various places (S3, DynamoDB, RDS, RedShift, etc.)

`5` transactions per second for reasons, up to a maximum total data rate of `2MB/sec`. Up to `1000` records per second for writes, up to a toal maximum data write rate of `1MB/sec` (including partition keys).

Each shard can achive these capacities, total capacity is a sum of the shards.

>Note: Kinesis Streams is the only form of kinesis that has shards.

## Kinesis Firehose
Producers stream data to Kinesis. No **persistent storage**, data must be handled/analyzed as it comes in (typically with Lambda functions). Analytics are output to S3, RedShift (via S3), or ElasticSearch Cluster.

Consists of delivery streams, records of data and destinations.

## Kinesis Analytics
Works with Kinesis Streams or Kinesis Firehose, on-the-fly analytics that then can store the analytic data in various places (S3, DynamoDB, RDS, RedShift, etc.).

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
- Kinesis Data Streams
- Kinesis Firehose
- Kinesis Analytics

## Kinesis Data Streams (KDS)
Producers stream data to Kinesis. Kinesis by default stores data for 24 hours to 7 days based on configuration. 

Data streams are sharded to increase throughput and scalability, partition keys are assigned to shards and determine which shard a record is added.

Each shard can support `5` read transactions per second, up to a maximum total data rate of `2MB/sec`. Up to `1000` records per second for writes (PUTs), up to a toal maximum data write rate of `1MB/sec` (including partition keys).

Total capacity of a data stream is a sum of the shards.

>Note: Kinesis Streams is the only form of kinesis that has shards.

## Kinesis Firehose
Producers stream data to Kinesis. No **persistent storage**, data can be transformed with Lambda functions as it comes in. Data is then output to S3, RedShift (via S3), Amazon Elasticsearch or Splunk.

Consists of delivery streams, records of data and destinations.

`2000 transactions/second`, `5000 records/second`, `5MB/sec` - limit can be increased by asking AWS.

## Kinesis Data Analytics
Works with Kinesis Streams or Kinesis Firehose, on-the-fly analytics that then can store the analytic data in various places (S3, DynamoDB, RDS, RedShift, etc.). Kinesis Data Analytics **applications** are configured with `input`, `application code` and `output` to perform a given analytic task.

Used for:
- Streaming ETL
- Continuous metric generation
- Responsive real-time analytics
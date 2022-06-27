# Kennisis
Amazon Kinesis is the AWS solution for collecting, processing, and analyzing streaming data in the cloud. When you need "real-time" think Kinesis.   
- Kinesis Data Streams Per per running shard, data can persist within the stream, data is ordered and every consumer keep its own position. Consumers have to be manually added (coded), Data persists for 24 hours (default) to 168 hours.
- Kinesis Firehose - Pay for only the data ingested, data immediately disappears once processed.
- Consumer of choice is from a predefined set of services: S3, Redshift, Elasticsearch or Splunk
- Kinesis Data Analytics - allows you to perform queries in real-time. Needs a Kinesis Data   
- Streams/Firehose as the input and output.
- Kinesis Video Analytics securely ingests and stores video and audio encoded data to consumers such as SageMaker, Rekognition or other services to apply Machine learning and video processing.
- KPL (Kinesis Producer Library) is a Java library to write data to a stream
- You can write data to stream using AWS SDK, but KPL is more efficient
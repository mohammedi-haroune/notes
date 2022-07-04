# SQS
SQS is a queuing service using messages With a queue. Think Sidekiq or RabbitMQ   
- SQS is used for Application Integration, it lets decoupled services and apps to talk to each other   
- To read SQS use need to pull the queue using the AWS SDK. SQS is not pushed-based   
- SQS supports both Standard and First-ln-First-Out (FIFO) queues   
- Standard allows nearly unlimited messages per second, does not guarantee order of delivery, always delivers at least once, you must protect again duplicate messages being processed
- FIFO maintain the order of messages With a 300 messages per second limit   
- Short polling returns messages immediately, even if the message queue being polled is empty.   
- Long polling waits until message arrives in the queue, or the long poll timeout expires. ln majority of cases Long polling is preferred over short polling. its less expensive since it avoids empty reads
- Visibility time-out is the period of time that messages are invisible in the SQS queue   
- Messages will be deleted from queue after a job has processed. (before visibility timeout expires)   
- If Visibility timeout expires than a job will become visible to the queue   
- The default Visibility time-out is 30 seconds. Timeout can be 0 seconds to a maximum of 12 hours.   
- SQS can retain messages from 60 seconds to 14 days and by default is 4 days   
- Message size between 1 byte to 256 kb, Extended Client Library for Java can increase to 2GB

# SNS
Simple Notification Service (SNS) is a fully managed pub/sub messaging service   
- SNS is for Application Integration. It allows decoupled services and apps to communicate With each other   
- Topic a logical access point and communication channel.   
- A topic is able to deliver to multiple protocols   
- You can encrypt topics via KMS   
- Publishers use the AWS API via AWS CLI or SDK to push messages to a topic. Many AWS services integrate With SNS and act as publishers.   
- Subscriptions subscribe to topics. When a topic receives a message it automatically and immediately pushes messages to subscribers.   
- All messages published to SNS are stored redundantly across multiple Availability Zones (AZ). 
- The following protocols:   
	- HTTPs and HTTPs create webhooks into your web-application
	- Email good for internal email notifications (only supports plain text)   
	- Email-JSON sends you json via email   
	- Amazon SQS place SNS message into SQS queue   
	- AWS Lambda triggers a lambda function   
	- SMS send a text message   
	- Platform application endpoints Mobile Push eg. Apple, Google, Microsoft Baidu notification systems
	
	
# ElastiCache
Manage cache in-memory Data store like Memcached and Redis
- Memcached is a simple key / value store preferred for caching HTML fragments and is arguably  faster than Redis   
- Redis has richer data types and operations. Great for leaderboard, geospatial data or keeping track  of unread notifications.   
- Most frequently identical queries are stored in the cache   
- Resources only within the same VPC may connect to ElastiCache to ensure Iow latencies.
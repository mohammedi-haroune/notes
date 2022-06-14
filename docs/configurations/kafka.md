# Kafka

## Consumer Consumption Status
```bash
kafka-consumer-groups.sh --bootstrap-server kafka:9092 --describe --group test-migration | awk '{lag_sum += $5; total_sum += $4; done_sum += $3} END {print "Completed count: " done_sum ", Lag: " lag_sum ", Total number of records: " total_sum}'
```

## Get number of messages in a topic
```bash
kafka-run-class.sh kafka.tools.GetOffsetShell --broker-list localhost:9092 --topic mytopic --time -1 --offsets 1 | awk -F ":" '{sum += $3} END {print sum}'
```

## View the details of a consumer group
```bash
kafka-consumer-groups.sh --bootstrap-server kafka:9092 --describe --group <group name>
```

## List the consumer groups known to Kafka
```bash
kafka-consumer-groups.sh --bootstrap-server localhost:9092 --list
```

# Kafka Consumers: Reading Data from Kafka
## Kafka Consumer Concepts
### Consumers and Consumer Groups
Kafka consumers are typically part of a `consumer group`. When multiple consumers are subscribed to a topic and belong to the same consumer group, each consumer in the group will receive messages from a different subset of the partitions in the topic.

If we add more consumers to a single group with a single topic than we have partitions, some of the consumers will be idle and get no messages at all.

To summarize, you create a new consumer group for each application that needs all the messages from one or more topics. You add consumers to an existing consumer group to scale the reading and processing of messages from the topics, so each additional consumer in a group will only get a subset of the messages.

### Consumer Groups and Partition Rebalance
Rebalance is the process of changing the ownership of a topic from consumer to another, it happens when a consumer cashes, when a new consumer start consuming or when the topic is modified (eg. the number of partitions is modified), during rebalance the entire consumer groups can't consumer the messages, it causes a short window of **unavailability**.



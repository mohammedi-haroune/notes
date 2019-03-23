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

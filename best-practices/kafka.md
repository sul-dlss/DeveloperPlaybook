# Kafka tips

First ssh to the machine:
```
indexer@sul-kafka-stage-a
```

Then list the topics/groups/consumers
```
/opt/kafka/bin/kafka-consumer-groups.sh --bootstrap-server localhost:9092 --describe --all-groups --all-topics
```

Or just the topics/consumers for a known group
```
/opt/kafka/bin/kafka-consumer-groups.sh --bootstrap-server localhost:9092 --describe --group traject_folio_dev
```


Show some of the messages:
```
/opt/kafka/bin/kafka-console-consumer.sh --bootstrap-server localhost:9092  --topic purl_fetcher_stage --offset 288005 --partition 0
```


To change the offset (where :7 in the topic indicates the partition):
```
/opt/kafka/bin/kafka-consumer-groups.sh --bootstrap-server localhost:9092 --group purl-updates-consumer --reset-offsets --topic purl-updates:7 --to-offset 695369 --execute
```

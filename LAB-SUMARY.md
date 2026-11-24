# Lab Summary

Commands used:

```powershell
docker compose up -d
docker compose exec kafka kafka-topics --bootstrap-server localhost:9092 --create --if-not-exists --topic transactions
docker compose exec kafka kafka-topics --bootstrap-server localhost:9092 --describe --topic transactions
docker compose exec kafka kafka-console-producer --bootstrap-server localhost:9092 --topic transactions
docker compose exec kafka kafka-console-consumer --bootstrap-server localhost:9092 --topic transactions
Get-Content .\transactions.csv | Select-Object -Skip 1 | docker compose exec -T kafka kafka-console-producer --bootstrap-server localhost:9092 --topic transactions
```

The topic had one partition. Each producer input line became a separate record, and my consumer displayed two messages before I stopped it. A consumer group resumes from its committed offset; a new group with `--from-beginning` can read older records.

Topics are logical streams: producers and consumers exchange related records through a named topic without needing to know about each other. Partitions let Kafka distribute a topic's records across brokers and enable parallel processing, while preserving order within each partition. Offsets mark a consumer group's position in a partition, allowing it to track progress and resume. Within a consumer group, each partition is assigned to at most one consumer at a time, preventing multiple group members from processing that partition concurrently. If a group has more consumers than partitions, the extra consumers remain idle.

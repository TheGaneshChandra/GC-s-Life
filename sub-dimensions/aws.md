# AWS

## Kinesis

### Kinesis Data stream
> Near real time bits of data transfer from PRODUCER to CONSUMER
- Streams
    |
    |-- Shards <!-- Different threads based on partition key of data in a stream -->
        |
        |-- SequenceNumber <!-- A unique number that gets incremented for each record in shard -->
        |
        |-- ShardIterator <!-- A pointer from where the shard data is read forward -->
            |- "TRIM HORIZON" <!-- Starts from the oldes record -->
            |- "LATEST" <!-- Starts from the latest -->
            |- "AT_SEQUENCE_NUMBER" <!-- Starts from a given sequence number -->
            |- "AFTER_SEQUENCE_NUMBER" <!-- Starts from the next record of given seq number -->
            |- "AT_TIMESTAMP" <!-- Starts from the given timestamp-->
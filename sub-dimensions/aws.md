# AWS

## Kinesis

### Kinesis Data stream
> Near real time bits of data transfer from PRODUCER to AWS which a PRODUCER can consume
- Streams
    |
    |-- Shards <!-- Different threads based on partition key of data in a stream -->
        |
        |-- Producer <!-- Any device or application that sends data to Kinesis data streams -->
        |   |
        |   |-- Send One record per call <kinesis.put_record( StreamName="", Data="", PartitionKey="")>
        |   |-- Send Batch upto 500 <kinesis.put_records( StreamName="", Records=[ {"Data":, "PartitionKey":} ])>
        |
        |-- SequenceNumber <!-- A unique number that gets incremented for each record in shard -->
        |
        |-- PartitionKey <!-- A column used to define which shard the data goes to -->
        |   |
        |   |-- MD5_Hashing <!-- A type of hash that gives values in range 0-2^128, Used to differentiate unique partition keys -->
        |
        |-- ShardIterator <!-- A pointer from where the shard data is read forward -->
        |   |
        |   |-- "TRIM HORIZON" <!-- Starts from the oldes record -->
        |   |-- "LATEST" <!-- Starts from the latest -->
        |   |-- "AT_SEQUENCE_NUMBER" <!-- Starts from a given sequence number -->
        |   |-- "AFTER_SEQUENCE_NUMBER" <!-- Starts from the next record of given seq number -->
        |   |-- "AT_TIMESTAMP" <!-- Starts from the given timestamp-->
        |
        |-- Retention_Period <!-- Amount of time Data is alive in data stream before getting deleted (default: 24hours, max: 365days) -->
        |
        |-- Checkpointing <!-- Saves the last sequenceNumber so to process only the next records handled by user or using KCL -->
        |
        |-- Resharding <!-- Splitting or Merging of Shards based on Throughput limit (write: 1MB/sec, 1000 records/sec) or (read: 2 MB/sec, 5 Get_record() calls -->
        |
        |-- Provisioned_Mode <!-- AWS handling the Automatic scaling (Resharding) of shards based on data throughput-->
        |
        |-- Consumers <!-- Any Application that reads data from Kinesis data streams -->
        |   |
        |   |-- Classic <GetRecords(shard_iterator)> <!-- Total of 2MB/sec Bandwidth for all consumers with around 200ms latency -->
        |   |-- Enhanced_Fan_Out (EFO) <SubscribeToShard()> <!-- Each consumer gets 2MB/sec bandwidth with 70ms latency -->
        |
        |-- Kinesis_Client_Library (KCL) <!-- A library that handles some tasks related to Kinsis -->
            |
            |-- Checkpointing <checkpoint(sequence_number)> <!-- Saves the last sequenceNumber so to process only the next records (stored in dynamodb) -->
            |-- Shard_Discovery
            |-- LoadBalancing
            |-- Fault recovery
            |-- ExcatlyOnceProcessing



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


### Kinesis FireHose
> Fully managed streaming service that **delivers** data to S3, RedShift, OpenSearch, Splunk, Snowflake
- Delivery Stream <!-- The stream that moves the data from producer to the consumer destination -->
    ```python
        
        import boto3
        firehose = boto3.client("firehose")
        firehose.create_delivery_stream(
            DeliveryStreamName="",
            S3DestinationConfiguration = {}
        )

        # send record
        firehose.put_record(
            DeliveryStreamName = "",
            Record = [{}]
        )
    ```
    |
    |-- Buffering <!-- Firehose delivers data in a batch format, so if buffers the stream data based on chosen size(1MB-128MB), interval(1-900sec) -->
    |
    |-- Data Transformtion using Lambda <!-- Lambda function to be executed on each batch before putting into Consumer destination -->
    |
    |-- Compression Formats <!-- Can Compress before writing to destination (GZIP, Snappy, ZIP) -->
    |
    |-- Data Formats <!-- Can convert the data to desired format (JSON, PARQUET, ORC) before writing  -->
    |
    |-- S3 backup mode <!-- if writing to destination fails, will save the data in this backup bucket -->
    |
    |-- Auto Retires
    |
    |-- AutoScaling

### AWS Managed Apache Flink
> A distributed processing engine for stateful computations of bounded and unbounded data
- Stateful Stream processing <!-- Remembers past events, Keep local state on each operator checkpointed to S3/Dynamodb-->
- Event-Time processing <!-- Processes events when happened, not when arrived -->
- Exaclty once Stateful processing Guarenteed <!-- Industry standard for exactly once processing -->
- Windowing functionaliy <!-- events grouped by time or count -->
- Fault Tolerance, Savepointing <!-- Can restart entire pipeline without losing state -->
- Batch + Streaming

### AWS Managed Kafka
> Distributed log-based Event streaming platform used for messaging, **event storage**, and streaming analytics
- Durable storage layer <!-- Can store data upto years, replay history at any time -->
- Stream Replay (Time travel) <!-- Consume old data at any offset by any consumer -->
- Independent Consumers <!-- Consumers can access, replay data independently -->
- Extremely High-throughput <!-- Millions per second -->
- Kafka Connect <!-- can be connected to many connectors with no code -->

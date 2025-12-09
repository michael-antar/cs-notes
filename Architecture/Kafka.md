A *distributed event streaming platform* that handles high-throughput, real-time data feeds.

It is a **distributed commit log**:
- **Storage**: It stores data as an append-only sequence of records on the disk.
- **Persistence**: Unlike many queues that hold messages in RAM, Kafka writes everything to the hard drive immediately.

## Topics / Consumer Groups
Kafka implements the [[Pub Sub Model|Pub Sub Model]], but adds a layer of concurrency called *consumer groups*
- **Topics**: A named *stream of records*. Producers write to a topic; Consumers read from it.
- **Consumer Groups** (The Concurrency Model):
	- If you have a topic `telemetry`, you can have a "Service A" group and a "Service B" group
	- Both groups receive a full copy of the data (Pub/Sub)
	- Within "Service A", you can have 10 instances. Kafka divides the data so that each instance processes a unique subset of the data ([[Load Balancer|Load Balancing]])

## Optimization
Kafka is optimized for *high throughput* (millions of writes per second) using 
two primary mechanisms:
- **Sequential I/O (Disk Mechanics)**: Kafka does not use random access. It strictly *appends* to the end of a file.
	- Modern HDDs/SSDs are extremely fast at sequential writes
	- Kafka writes are practically limited only by the network speed, not the disk speed
- **Partitioning (Parallelism)**: A Topic is a logical concept. Physically, _a Topic is split into partitions_
	- If a Topic has 10 Partitions, the data is split across 10 different files, potentially on 10 different servers (Brokers)
	- This allows 10 writers and 10 readers to operate exactly at the same time without locking each other
	- _Order is only maintained within a Partition_. If Partition 1 has Message A, and Partition 2 has Message B, Kafka cannot guarantee A is read before B. If you send Message A and Message B to Partition 1, you will always read A then B.
		- If strict ordering is needed for a specific device, you must ensure all messages for `Device ID 123` are sent to the _same partition_. You do this by using the Device ID as the "Partition Key" when producing the record.

## Management
*Kafka does not pull, Consumers pull*.
- **The Pull Model**: The Kafka Broker does not force data upon the Consumer. The *Consumer requests data* ("Give me the next 10 records.")
- **Blocking**: If the consumer takes 5 minutes to process Record #1, it simply won't request Record #2 until it is done. Kafka will wait indefinitely (subject to data retention limits).
- **Management**: It is the *responsibility of the Consumer application code* to implement timeouts or move "bad" messages to a "Dead Letter Queue" if they fail to process, so they don't block the stream.
## Comparison
Both Kafka and [[AWS SQS]] are asynchronous messaging tools, but their fundamental behavior is opposite.

| Feature        | AWS SQS (Queue)                                                                       | Kafka (Log)                                                                                                                             |
| -------------- | ------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| Data Lifecycle | **Destructive Read**. Once a consumer acknowledges a message, it is deleted from SQS. | **Non-Destructive Read**. Reading a message does not delete it. It stays on the disk until the retention period (e.g., 7 days) expires. |
| Consumers      | Competitors. Only one consumer processes a message.                                   | Multi-Subscriber. Multiple different applications can read the exact same message history independently.                                |
| Use Case       | Task Processing (Send email, resize image)                                            | Event Streaming (Telemetry, Activity Tracking, Sourcing)                                                                                |


A *distributed event streaming platform* that handles high-throughput, real-time data feeds.

It is a **distributed commit log**:
- **Storage**: It stores data as an append-only sequence of records on the disk.
- **Persistence**: Unlike many queues that hold messages in RAM, Kafka writes everything to the hard drive immediately.

Kafka implements the [[Pub Sub Model|pub sub model]], but adds a layer of concurrency called *consumer groups*
- **Topics**: A named *stream of records*. Producers write to a topic; Consumers read from it.
- **Consumer Groups** (The Concurrency Model):
	- If you have a topic `telemetry`, you can have a "Service A" group and a "Service B" group
	- Both groups receive a full copy of the data (Pub/Sub)
	- Within "Service A", you can have 10 instances. Kafka divides the data so that each instance processes a unique subset of the data ([[Load Balancer|Load Balancing]])

Kafka is optimized for *high throughput* (millions of writes per second) using 
two primary mechanisms:
- **Sequential I/O (Disk Mechanics)**: Kafka does not use random access. It strictly *appends* to the end of a file.
	- 

## Questions
- What is the point of consumer groups? Why would you want groups that receive the same copies of the data? Does each instance have a unique piece of the data with no overlaps?

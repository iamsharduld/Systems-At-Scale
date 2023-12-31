# Scalable Systems Reference Guide

Scalable systems are designed to handle an increasing amount of work by adding resources to the system. Below is a collection of key concepts and methodologies essential for designing and understanding scalable systems.

## Table of Contents

1. [Load Balancing](#load-balancing)
2. [Distributed Databases](#distributed-databases)
3. [Distributed Messaging](#distributed-messaging)
4. [Caching Strategies](#caching-strategies)
5. [Fault Tolerance](#fault-tolerance)
6. [Monitoring and Logging](#monitoring-and-logging)

### Load Balancing

Load balancing distributes network or application traffic across multiple servers, ensuring that no single server bears too much demand.

- **Strategies**: Round Robin, Least Connections, IP Hash
- **Session Persistence**: Techniques to ensure that a user's consecutive requests are sent to the same server.
- **Health Checks**: Monitoring the health of the servers to avoid sending requests to failed or underperforming servers.

### [Distributed Databases](./distributed_databases/)

Distributed databases spread data across multiple physical locations. They are designed to improve performance, availability, and fault tolerance.

- **Types**: Horizontal Partitioning, Vertical Partitioning
- **CAP Theorem**: Balancing Consistency, Availability, and Partition tolerance.
- **Database Systems**: Cassandra, MongoDB, MySQL Cluster
- **Replication and Sharding** :Replication ensures that data is duplicated across multiple nodes, while sharding partitions data to spread it across servers.

  - **Replication Types**: Master-Slave, Peer-to-Peer
  - **Sharding Strategies**: Range-based, Directory-based, Hash-based

### Distributed Messaging

Distributed messaging systems facilitate communication between different parts of a scalable system, ensuring high throughput and fault tolerance.

- **Messaging Patterns**: Publish-Subscribe, Queuing
- **Brokers**: [Kafka](./distributed_messaging/kafka.md), RabbitMQ, ActiveMQ
- **Delivery Guarantees**: At-most-once, at-least-once, exactly-once delivery
- **Fault Tolerance**: Techniques for recovering from failures
- **Scalability**: Ability to grow by adding more components

### [Caching Strategies](./cache/)

Caching improves system performance by storing instances of data in high-speed storage areas.

- **Types**: In-memory caches, [Content Delivery Networks (CDNs)](./cache/CDN.md)
- **Cache Eviction Policies**: LRU, FIFO, LFU

### [Fault Tolerance](./fault_tolerance/)

Fault tolerance ensures that a system continues to operate even if some components fail.

- **Techniques**: Redundancy, Failover, Graceful Degradation

### [Monitoring and Logging](./monitoring_and_logging/)

Effective monitoring and logging provide insights into system performance, security, and user behavior.

- **Tools**: Prometheus, Grafana, ELK Stack
- **Metrics**: CPU Utilization, Memory Usage, Error Rates

---

*Note: This guide is meant as an overview and starting point. Further reading and hands-on practice are encouraged for a deeper understanding of these concepts.*


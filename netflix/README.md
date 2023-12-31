# Netflix System Architecture

Netflix's architecture is a remarkable example of scalable, resilient, and user-focused design. This document provides an overview of key components and principles that govern the system's operation.

## [Content Delivery Network (CDN)](./CDN/README.md)

- **Netflix Open Connect**: Custom CDN for global streaming content distribution.
- **Placement in ISPs**: Servers inside ISPs' data centers to reduce latency and increase throughput.

## Microservices Architecture

- **Decoupled Services**: Functionalities as small, autonomous services.
- **Independent Scaling**: Services scaled independently based on demands.

## Data Storage

- **Cassandra**: For distributed and available storage.
- **MySQL**: For customer and billing data.
- **S3**: For backup and archival.

## Streaming Control Plane

- **Playback**: Determines best CDN and bitrate for streaming.
- **Adaptive Streaming**: Adjusts quality in real-time based on user's network.

## Recommendation Engine

- **Machine Learning Algorithms**: Personalized recommendations.
- **Real-Time Processing**: Apache Kafka for real-time data processing.

## User Interface & Experience

- **API Gateway**: Central entry point for requests.
- **A/B Testing**: Constantly tests different UI versions.

## Resilience & Redundancy

- **Multi-Region Deployment**: High availability and disaster recovery.
- **Chaos Engineering**: Tools like Chaos Monkey for resilience.

## Monitoring & Analytics

- **Real-Time Monitoring**: Constant system health and performance monitoring.
- **Big Data Analytics**: Processing and analyzing data for insights using Apache Spark.

## Security

- **Content Encryption**: Secure content delivery.
- **Authentication & Authorization**: User data and privacy protection.

## Technologies & Tools

- Java, JavaScript, React, Spring Boot, Docker, Kubernetes, etc.

## References

* https://openconnect.netflix.com/en/
* https://teamresellerclub.medium.com/how-the-cloud-and-cdn-architecture-works-for-netflix-8f3d17906782

## To Do
1. Add the system architecture diagram
2. Add References


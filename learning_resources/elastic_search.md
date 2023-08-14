# Elasticsearch: Architecture, Uses, and Companies

## Table of Contents
1. [Architecture](#architecture)
2. [Uses](#uses)
3. [Companies Using Elasticsearch](#companies-using-elasticsearch)

## Architecture <a name="architecture"></a>
Elasticsearch is a distributed search and analytics engine built on Apache Lucene. Here's an overview of its architecture:

### Indices
- An index is a collection of documents sharing the same data structure.
- Each index is subdivided into **shards**, providing horizontal scalability.
- Shards are distributed across different nodes in the cluster to provide fault tolerance.

### Nodes and Clusters
- A **node** is a running instance of Elasticsearch, and multiple nodes form a **cluster**.
- Nodes are categorized into roles such as master, data, ingest, etc.
- **Master nodes** control the cluster, while **data nodes** store data.

### Search and Analytics
- Elasticsearch's core functionality is full-text search, allowing complex queries and aggregations.
- Utilizes an [inverted index](./misc/inverted_index.md) to enable efficient search operations.

## Uses <a name="uses"></a>
Elasticsearch is widely used across various domains:

1. **Full-Text Search**: Enables powerful search functionality within large datasets.
2. **Log and Event Data Analysis**: Often used in monitoring tools for real-time analysis.
3. **Data Visualization**: Integrated with tools like Kibana to create dynamic dashboards.
4. **Real-Time Analytics**: Suitable for performing real-time analytics on streaming data.

## Companies Using Elasticsearch <a name="companies-using-elasticsearch"></a>
Several renowned companies leverage Elasticsearch in their technology stacks:

1. **Netflix**: For real-time monitoring and analysis of viewing activities.
2. **Uber**: Used in Uber's logging platform for querying log data.
3. **Slack**: Employs Elasticsearch to enable full-text search within messages.
4. **LinkedIn**: Utilizes Elasticsearch to handle various search and analytics use cases.
5. **eBay**: Employs Elasticsearch for search functionality across its massive product catalog.

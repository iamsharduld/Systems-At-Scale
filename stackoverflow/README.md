![image](https://github.com/iamsharduld/Systems-At-Scale/assets/8417988/722a9e4d-8568-48b1-8e2d-b3bc2ebc4811)

# Network Setup

### 1. DNS (Domain Name System):
  * **CloudFlare**: Stack Overflow uses CloudFlare for DNS hosting, which translates human-readable domain names into IP addresses. CloudFlare's global presence ensures quick DNS resolution.
  * **In-house DNS Servers**: They also maintain their own DNS servers as a backup, to provide redundancy and maintain control.

### 2. ISPs (Internet Service Providers):
  * **Four ISPs**: Traffic comes through four different ISPs (Level 3, Zayo, Cogent, and Lightower) in New York, adding redundancy and optimal routing.

### 3. Edge Routers:
  * **ASR-1001 and ASR-1001-X Routers**: These are the routers that manage incoming traffic from the ISPs, set up in two pairs, each handling two ISPs.
  * **BGP (Border Gateway Protocol)**: Used for routing between different networks (ISPs) to control traffic flow and optimize paths.
  * **VLANs (Virtual Local Area Networks)**: These separate and isolate external traffic.

### 4. Load Balancers:
  * After the routers, traffic is directed to load balancers that distribute the load across servers, ensuring efficient utilization and redundancy.

### 5. 10Gbps MPLS (Multiprotocol Label Switching) between Data Centers:
  * This high-speed connection between two data centers is used for data replication and quick recovery but not directly for serving the site.
  * **Redundancy Concern**: Although the MPLS connection seems to be a single point of failure, they have additional failover routes through OSPF (Open Shortest Path First) via their ISPs to provide backup connections.
  * **Connectivity to Colorado**: Mentioned are connections to another location in Colorado, presumably another data center, with failover routing between them.

# Load Balancers

## 1. Load Balancer Software: 
HAProxy 1.5.15 on CentOS 7.
## 2. TLS Termination: 
Managed within HAProxy.
## 3. Network Connections: 
Two pairs of 10Gbps connections for each load balancer, designated for the external network and DMZ.
## 4. Memory Utilization: 
64GB or more in each load balancer to efficiently cache TLS sessions.
## 5. Session Resumption: 
Caching allows for faster and more cost-effective connection resumption.
## 6. 
Additional Features: Includes rate limiting and header captures for performance metrics.

# Web Tier

## 1. Software Stack: 
IIS 8.5, ASP.Net MVC 5.2.3, and .Net 4.6.1.
## 2. Web Servers: 
9 "primary" servers (01-09) for main sites and 2 "dev/meta" servers (10-11) for staging.
## 3. Distribution: 
Primary servers handle sites like Stack Overflow and Careers, while meta sites run on the last two servers.
## 4. Multi-tenant Application: 
A single application serves requests for all Q&A sites, allowing the entire Q&A network to run off a single application pool on one server. Other applications like Careers and APIs are separate.
## 5. Monitoring: 
They use Opserver, an internal monitoring dashboard, to observe Stack Overflow's distribution and utilization across the web tier.
## 6. Overprovisioning: 
The servers are overprovisioned for purposes like rolling builds, headroom, and redundancy, with further details to be discussed in future posts.

# Service Tier

## 1. Software and Platform
Similar to the web tier, the service tier runs on IIS 8.5, ASP.Net MVC 5.2.3, .Net 4.6.1, and includes HTTP.SYS, all on Windows 2012R2.

## 2. Function
It operates internal services to support the web tier and other internal systems.

## 3. Components
  * **Stack Server**: Manages the tag engine, and is based on http.sys (outside of IIS). It refreshes question lists every 2 minutes, impacting the cache, so affinity must be set for separate sockets.
  * **Providence API**: IIS-based component.

## 4. Redundancy and Efficiency
Unlike the 9-fold redundancy in the web tier, the service tier emphasizes efficiency and has lesser redundancy. Data loading from the database is done only three times for cost efficiency but still maintains safety.

## 5. Hardware Configuration
The boxes are configured to be optimized for different computational loads, like the tag engine and elastic indexing jobs.

## 6. Tag Engine
A crucial component handling all tag matching outside of /search, enabling features like new navigation. A more detailed explanation is reserved for a dedicated post.

# Cache & Pub/Sub

## 1. Redis Usage
Utilized as a rock-solid system for about 160 billion operations per month, consuming less than 2% CPU.

## 2. L1/L2 Cache System
  * **L1**: HTTP Cache on the web servers or related application.
  * **L2**: A fallback to Redis for fetching values.
  * **Operation**: On a cache miss in both levels, a value is fetched from source (e.g., database, API) and placed in both local cache and Redis. Subsequent servers can find the value in L2, saving additional queries.

## 3. Site-Specific Caching
Each Q&A site has its own L1/L2 caching system, identified by key prefix and database ID.

## 4. Main Redis Servers
2 primary servers (master/slave) and additional machine learning instances (for recommendations, matching jobs, etc.), known as Providence.

## 5. Memory
Main servers have 256GB of RAM (90GB in use); Providence servers have 384GB of RAM (125GB in use).

## 6. Protobuf Format
Values are stored in Protobuf format, using specific in-house and open-source libraries.

## 7. Publish & Subscribe Mechanism
Beyond caching, Redis also supports a mechanism for messaging, allowing one server to publish a message and all subscribers to receive it. Used for clearing caches on other servers for consistency and other uses like websockets.

# Web Sockets

## 1. Purpose
Websockets are used to push real-time updates to users, including notifications, vote counts, new answers, comments, and other real-time data.

## 2. Implementation
The socket servers utilize raw sockets running on the web tier and are built on the open-source library, StackExchange.NetGain.

## 3. Concurrency
At peak times, there can be about 500,000 concurrent websocket connections open.

## 4. Efficiency
Compared to polling, websockets are much more efficient at Stack Overflow's scale, enabling the push of more data with fewer resources, and providing instantaneous updates to users.

## 5. Challenges
Despite their efficiency, websockets are not without issues, such as the exhaustion of ephemeral ports and file handles on the load.

## 6. Interesting Fact
Some browsers with websocket connections have been open for over 18 months, leading to humorous speculation about the status of those developers.

# Search

## 1. Usage
Elasticsearch is used for handling search functionality on the site, including performing searches, calculating related questions, and offering suggestions when asking a question.

## 2. Implementation
The searches are conducted against Elasticsearch 1.4, using a specialized client called StackExchange.Elastic, which exposes only a specific subset of the API Stack Overflow uses.

## 3. Cluster Configuration
Each Elastic cluster has 3 beefier-than-average nodes, with all SSD storage, 192GB of RAM, and dual 10Gbps network each. Indexing is also continually handled by the same domains that host the tag engine.

## 4. Reason for Using Elasticsearch
  * **Scalability**: Compared to other options like SQL full-text search, Elasticsearch is more scalable.
  * **Cost-Effectiveness**: SQL CPUs are more expensive, while Elastic is comparatively cheap and provides more features.
  * **Cross-Network Search**: Stack Overflow needed to search across many indexes at once, something that wasn't supported by other solutions like Solr at the time of decision-making.

## 5. Challenges
Upgrading to a newer version of Elasticsearch is hindered by a major change to "types," requiring a complete reindexing of everything and a migration plan.

# Databases

## 1. Single Source of Truth
SQL Server is the single source of truth, with all data in Elastic and Redis originating from it. This decision simplifies data consistency across different platforms.

## 2. Cluster Configuration
Two clusters are maintained, each with 1 master and multiple replicas, including one in a disaster recovery data center.
  * **First Cluster**: Dell R720xd servers hosting databases like Stack Overflow, Sites, PRIZM, and Mobile.
  * **Second Cluster**: Dell R730xd servers running everything else, such as Talent, Open ID, Chat, Exception log, and other Q&A sites.

## 3. Hardware Specification
The servers have substantial RAM and SSD space, which caters to the demanding requirements of the site.

## 4. Low CPU Utilization
They aim to keep CPU utilization low, though there are current issues related to plan cache that are being addressed. This ensures that the databases run efficiently and have room for spikes in demand.

## 5. Usage Simplicity
The usage of SQL is deliberately kept simple.
  * **Fast Interaction**: Simple queries and interactions lead to faster execution.
  * **Technology Choice**: Legacy Linq2Sql is present, but new development uses Dapper, an open-source Micro-ORM.
  * **Minimal Use of Stored Procedures**: There's only one stored procedure, with an intention to move even that into code, allowing for more control and maintainability in the application itself.

# Disclaimer

(TBD - Add any necessary disclaimers here)

# References
- **Starting Point** - [Nick Craver's Blog](https://nickcraver.com/blog/2016/02/03/stack-overflow-a-technical-deconstruction/)
- [**Stack Overflow Podcast**](https://stackoverflow.blog/podcast/): Sometimes they discuss technical aspects of their infrastructure on their podcast.
- [**Stack Overflow Engineering Blog**](https://stackoverflow.blog/engineering/): Various insights into their engineering decisions and architecture.
- [**Stack Exchange on GitHub**](https://github.com/StackExchange): Some of their code and tools are open-source, so you can explore them here.
- **ByteByteGo** - [YouTube Link](https://youtu.be/fKc050dvNIE)

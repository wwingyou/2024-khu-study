# 1st Week

## Chapter 01. Scalability according to the Number of Users

### Single Server

All of the components are executed in the same physical machine

Components

- Web App
- Database
- Cache
- etc..

Ruquest Flow

1. Users access the website through the domain name
    
    → DNS Server is needed and it is either own or 3rd-party service
    
2. Users get IP address by DNS server
3. Users send HTTP(or HTTPS) requests to the web server
4. Web server receives users’ request, processes it, and return the result
    
    → The result may be HTML page or JSON formatted response
    

### Database

As the number of users increases, a single server cannot ensure QoS

⇒ Each component is executed on individual physical machines

- Web App ⇒ Web/Mobile traffic handler (Web tier)
- Database ⇒ Database server (Data tier)
- Cache ⇒ Cache server (Cache tier)

**Types of Database**

- Relational DB (RDBMS)
    
    Data is represented in tables, rows, and columns
    
    - MySQL (recently, MariaDB)
    - Oracle Database
    - PostgreSQL
- Non-relataional DB (NoSQL)
    - Key-value Store — CouchDB, Aamzon DynamoDB
    - Graph Store — Neo4j
    - Column Store — Cassandra, HBase
    - Document Store

Relational databases are generally used

When NoSQL is used

- Very Low Latency (almost real time) is required
- Handling unstructured data
- Just serializing or deserializing data is needed
- Huge amount of data need to be stored

### Vertical Scaling VS Horizontal Scaling

|  | Vertical Scaling, Scale up | Horizontal Scaling, Scale out |
| --- | --- | --- |
| Definition | Adding high-spec resources(CPU, GPU, etc..) to physical machines | Adding more servers to physical machines |
| Pros | Simple | Can scale out more than scale-up
There is way of fialover and redundancy |
| Cons | Cannot scale up infinitely
There’s no way of failover or redundancy | Complex |
| Target | Small-scale applications | Large-scale applications |

**Load Balancer**

When there are too many users, distributing traffic evenly among web servers in a load balancing set prevents server downtime

Load balancing flow

1. Users access the web server using PUBLIC IP
2. Load balancer first receives packets and then selects one of the web servers in the load balancing set
    
    → There’s a lot of algorithms for selecting server
    
    1. Round Robin
    2. Least Connections
    3. IP Hash
    4. Least Response Time
    5. Weighted Round Robin
3. Load balancer forwards packets to the selected server using its PRIVATE IP
4. The web server receives users’ request, processes it, and return the result

⇒ No Failover & Increased Availability

**Database Clustering — Master-Slave**

Database is divided into MasterDB and SlaveDB

Write operations are only possible on the Master DB

Read operations are only possible on the Slave DB

Read operations typically occur more frequently ⇒ There are usually more Slave DBs

Advantages of database clustering

- Parallel processing becomes possible for more frequent Read operations, leading to improved performance
- Even if some databases go down, data is preserved (Reliability)
- Even if some databases go down, service will not go down (Availability)

### Cache

A mechanism for storing frequently referenced data in memory to reduce data movement time

Accessing the database every time a request occurs results in excessive latency

⇒ Use cache to reduce the number of database accesses

Caching flow (Read-through Caching Strategy)

1. Users send HTTP request to the web server
2. Web server receives users’ request, checks if the response is stored in cache
    1. If it’s stored, return the corresponding data to the users
    2. If it’s not, query the database
3. Web server returns the result

Cache server is accessed via an API

Considerations when using cache

- When is it desirable to use caching?
    
    → When frequent references occur rather than data updates
    
- What data should be cached?
    
    → Important data should be placed in a persistent data store
    
- How is cached data expired?
    
    → Appropriate expiration deadlines should be set
    
- How is consistency maintained?
    
    → The operation of updating the source and updating the cache should be processed as a single transaction
    
- How will failures be handled?
    
    → The cache servers should be distributed across multiple regions
    
- How should the cache memory size be determined?
    
    → Overprovisioning is advisable
    
- What is the data eviction policy?
    
    → Use LRU, LFU, FIFO, Round Robin, etc., according to the situation
    

### Content Delivery Network (CDN)

Geographically distributed servers' network used for transmitting static content

CND flow

1. Users access static content using url
    
    → Domain of URL is provided by CDN service provider
    
2. Check if the static content is stored in CDN server’s cache
    1. If it is, cache table is modified according to the eviction policy
    2. If it isn’t, retrieve static content from the origin server and cache it
3. CDN server returns static content to users

Considerations when using CDN

- Cost
    
    → Cache only frequently accessed content
    
- Expiration Deadline
    
    → Expiration time for time-sensitive content needs to be well-defined
    
- Plans for failures
    
    → If CDN itself goes down, clients should access the origin server directly
    
- Content Invalidation Method
    
    → CDN can remove content that has not yet expired
    

### Stateless Web Tier

To horizontally scale the web tier, state information needs to be removed from the web layer

(state information: session data, etc…)

⇒Store state information in a persistent storage and retrieve it when necessary

Why stateless?)

In a stateful web tier, requests from the same client should always be handled by the same server

→ adds burden to the load balancer and makes server addition and server failure handling more difficult

⇒ Use Stateless

### Data Center

To ensure a pleasant user experience and high availability worldwide

Place data centers in geographically distant locations and route user requests to the nearest data center ⇒ Geo-Routing, GeoDNS-Routing

If one data center goes down, all traffic is routed to the unaffected data center

Technical challenges for multi-data center architecture

- Traffic Routing
    
    Need the most efficient method for routing traffic to the correct data center
    
- Data Synchronization
    
    The contents of the database in each data center must be synchronized
    
- Test & Deployment
    
    Need to test the same application in multiple locations and require automated deployment tools
    

### Message Queue

Asynchronous communication methods that ensure message integrity

Preferable to build a stable application with loose coupling between servers to ensure scalability

→ Even if one of them dies, it can still send or receive messages

### Log, Metric, and Automation

- Log
    
    Collecting error log is important at both the server and service levels
    
- Metric
    
    Effective for collecting business status-related information or assessing the current system status
    
    Host-level Metrics: CPU, Memory, Disk I/O
    
    Aggregated Mertics: Performance of each tier
    
    Core Business Metric: Daliy active user, revenue, retention
    
- Automation
    
    Greatly improved development productivity
    

### Database Scalability

- Veritcal Scaling — Scale up
    
    Adding high-spec resources(CPU, GPU, etc..) to physical machines
    
- Horizontal Scaling — Scale out
    
    Adding more servers for performance improvement
    
    Sharding: Partitioning the database into smaller units called shards
    
    Selection of a sharding key(partition key) is crucial
    
    Need to choose a sharding key that evenly distributes the data
    
    Issues to address when implementing sharding
    
    - Resharding
        
        Needed when 1)data becomes too much for a single shard to handle, or when 2)data distribution between shards is uneven (Shard Exhaustion)
        
        Modify the function that calculates the shard key to redistribute the data
        
    - Celebrity Problem (Hotspot Key)
        
        The problem of query concentration on a specific shard causing server overload
        
        Assigning one shard to each hotspot key or splitting it into smaller pieces for better resolution
        
    - Join & De-normalization
        
        Sharding makes it difficult to join data across multiple shards
        
        Denormalizing the database to perform queries on a single table
        

### Summary

For Large-scale Application

- Web tier is stateless
- Ensure redundancy in all tiers
- Cache as much data as possible
- Support multiple data centers
- Serve static content through a CDN
- Scale the data layer through sharding
- Divide each tier into independent services
- Utilize continuous system monitoring and automation tools

![KakaoTalk_20240509_164930132](https://github.com/aws-cloud-clubs/2024-khu-study/assets/83760210/761f16e5-2721-477a-ad68-96afc8240547)

Overall Architecture

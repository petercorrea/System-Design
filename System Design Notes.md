---
tags: [Systems Design]
title: System Design Notes
created: '2021-05-06T00:39:59.085Z'
modified: '2021-05-08T06:11:14.086Z'
---

**System Design Notes**

-------------------------------------------------------------------------------------------------

# Distributed Systems
	- scalability
		- horizontal = more machines (easier)
		- vertical = better machines (harder)
	- reliability 
	- availability
	- efficiency 
	- manageable 

# Load Balancing 
	- Least Connections 
	- Least Response Time
	- Least Bandwidth
	- Round Robin
	- Weighted Round Robin
	- IP Hash

# Cache
	- Locality of Reference Principle
	- CDNs
	- Invalidation
		- write-through: write to cache and storage
			- pros: data consistency, minimize data lose
			- cons: higher write latency 
		- write-around: written to storage only
			- pros: fewer cache writes
			- cons: more cache miss on recent data
		- write-back: written to cache only, writes to storage is done later
			- pros: low latency for high write apps
			- cons: risk of data loss is higher
	- Eviction Policies
		- First In First Out (FIFO)
		- Last In First Out (LIFO)
		- Least Recently Used (LRU)
		- Most Recently Used (MRU)
		- Least Frequently Used (LFU)
		- Random Replacement (RR)

# Data Partitioning
    - Partitioning Schemes
        - Range/Horizontal: different rows into tables
        - Vertical: specific features into tables
        - Directory based: a lookup service which knows your current partitioning scheme and abstracts it away from the DB access code
        - Key or Hash-based partitioning: use consistent hashing
        - List partitioning
        - Round-robin partitioning
        - Composite partitioning: various methods
    - Common Problems
        - Joins and Denormalization
        - Referential Integrity 
        - Rebalacing 

  # Indexes
    - adding indexes is about improving the performance of search queries. If the goal of the database is to provide a data store that is often written to and rarely read from, in that case, decreasing the performance of the more common operation, which is writing, is probably not worth the increase in performance we get from reading.

  # Proxies
    - Open Proxy
        - Anonymous: Thіs proxy reveаls іts іdentіty аs а server but does not dіsclose the іnіtіаl IP аddress. Though thіs proxy server cаn be dіscovered eаsіly іt cаn be benefіcіаl for some users аs іt hіdes their IP аddress.
        - Transparent: Thіs proxy server аgаіn іdentіfіes іtself, аnd wіth the support of HTTP heаders, the fіrst IP аddress cаn be vіewed. The mаіn benefіt of usіng thіs sort of server іs іts аbіlіty to cаche the websіtes.
    - Reverse Proxy: retrieves resources on behalf of a client

  # System Redundancy and Data Replication
    -  improve reliability, fault-tolerance, or accessibility.

  # SQL nad noSQL
    - ACID: Atomicity, Consistency, Isolation, Durability
    - Storage Model, Schema, Querying, Scalability, ACID
    - SQL scales vertically better, ACID
      - Relational: MySQL, Oracle, MS SQL Server, SQLite, Postgres, and MariaDB.
    - noSQL scales horizontally better, no schema, not ACID
      - Key-Value: redis, voldemort, dynamo, memcache
        - fast, but limited space, no queries
      - Document: CouchDB, MongoDB, Firebase
        - no joins but scales well. Better for reading.
      - Wide-Column: Cassandra, HBase
        - good for unstructured data and scales horiz. but no joins
      - Graph: Neo4J, InfiniteGraph, GraphQL, 
      - Search Engine: ElasticSearch, Agolia
      - Multi Model: Fauna

  # CAP
    - Consistency, Avaliability, Partition tolerance 
    - We can have ONLY two of these
    - Availability + Consistency = RDBMS
    - Availability + Partition Tolerance = Cassandra, CouchDB
    - Consistency + Partition Tolerance = mongoDB, BigTable

  # Communication Protocols
    - Polling and Long Polling
    - WebSockets: full dupex, tcp
    - Server Sent Events: the client establishes a persistent and long-term connection with the server

# Steps to Design*
	- [ ] Define requirements and goal
	- [ ] Estimations and constraints 
		      - read and write traffic estimations
		      - storage for 5 years estimations  
		      - memory estimations/cache 80/20% rule
	- [ ] Define API
	      	- limit/monitor usage
	- [ ] Database Design
	      	- partitioning and replications 
	- [ ] Cache Design
	      	- tools: memcache
	      	- cache eviction policy 
	      	- cache invalidation
	- [ ] Load Balancer
	        - scheduling policy 
	- [ ] DB cleanup 
	- [ ] Telemetry
	- [ ] Security and Permissions 

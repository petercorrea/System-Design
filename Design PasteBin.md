---
tags: [Systems Design]
title: Design PasteBin
created: '2021-05-06T19:30:04.116Z'
modified: '2021-05-08T06:11:27.901Z'
---

**Design PasteBin**

# System Requierments and Goal
  - Functional Requirements
    1. Users should be able to upload or “paste” their data and get a unique URL to access it.
    2. Users will only be able to upload text.
    3. Data and links will expire after a specific timespan automatically
    4. Users should also be able to specify expiration time.
    5. Users should optionally be able to pick a custom alias for their paste.
  - Non-Functional Requirements
    1. The system should be highly reliable
    2. The system should be highly available
    3. Real-time with minimum latency.
    4. Paste links should not be guessable (not predictable).
  - Extended Requirements
    1. Analytics, e.g., how many times a paste was accessed?
    2. Our service should also be accessible through REST APIs by other services.
  - Design Considerations and Constraints
    1. Paste should be no more than 10MB
    2. Custom alias size limit


# Estimations
  - Assumptions
    - read-to-write ratio: 5:1
    - object max size: 10MB
    - metadate size: <1KB
    - writes: 1M/day

  - Calculations
    - Traffic
      - writes = 1M/secs in a day = 12 writes/sec
      - read = 5M/secs in a day = 58 reads/sec
      - average write: assume 10KB
        - 10KB * 1M/day = 10GB/day
    - Storage (5yrs)
      - objects: 10GB/day * 5 * 365 = 18TB/5 years
      - keys: 1M/day * 5 years = 1.8B/year
        - base64 encoded keys with 6 chars will be 64^6: 68.7 unique keys, enough
        - 68.7B * 6 bytes = 22GB for keys
    - Bandwidth
      - 12 writes/sec * 10KB = 120KB/sec
      - 58 reads/sec * 10KB = 0.6MB/sec
    - Cache (80-20 rule)
      - 0.2 * 5M * 10KB = 10GB


# API
  - addPaste(api_dev_key, paste_data, custom_url=None user_name=None, paste_name=None expire_date=None)
  - getPaste(api_dev_key, api_paste_key)
  - deletePaste(api_dev_key, api_paste_key)


# Database 
  - Observations: records are not related, billions of record, read heavy
  - metadata store and an object store
  - object store can be noSQL
  - metadata can be a relational store

   *Paradigm*
  	- [X] Relational
	  - [ ] Key-Value
    - [X] Document
    - [ ] Wide-Column
    - [ ] Graph
    - [ ] Search Engine
    - [X] Multi Model
    
  *Partitioning*
  	- [X] Range
    - [X] Hash
    - [ ] List
    - [ ] Round-robin
    - [ ] Composite

  *CAP*
    - [ ] Consistancy
    - [ ] Avaliability
    - [ ] Partition Tolerance
    
  - Communication Protocols
  - Replication and redundancy
  - Ceanup


# Cache
  *Invalidation*
		- [X] write-through
		- [ ] write-around
		- [ ] write-back
  
  *Eviction Policy*
  	- [ ] First In First Out (FIFO)
		- [ ] Last In First Out (LIFO)
		- [X] Least Recently Used (LRU)
		- [ ] Most Recently Used (MRU)
		- [ ] Least Frequently Used (LFU)
		- [ ] Random Replacement (RR)

  - Local or CDN?
  - Required space?


# Load Balancer
  *Location*
    - [ ] Client  - LB - App
    - [X] App     - LB - DB
    - [ ] DB      - LB - Cache
  
  *Scheduling*
    - [ ] Where to place
    - [ ] Scheduling Policy
    - [X] Least Connections 
    - [ ] Least Response Time
    - [ ] Least Bandwidth
    - [ ] Round Robin
    - [ ] Weighted Round Robin
    - [ ] IP Hash


# Telemetry
 

# Security and Permissions


# Design Components
  - Application Layer
    - How to handle read requests
    - How to handle write requests
    - Does it need key generation? 
    - cache keys?
    - Redundancy
  - Datastore Layer





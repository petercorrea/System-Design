---
tags: [Systems Design]
title: System Design Template
created: '2021-05-06T19:08:14.656Z'
modified: '2021-05-08T06:18:07.428Z'
---

# System Design Template


### System Requierments and Goal
  - Functional Requirements
  - Non-Functional Requirements
  - Extended Requirements
  - Design Considerations and Constraints 


### Estimations
  - Assumptions
    - read-to-write ratio:
    - reads:
    - writes:
    - object size:
  - Calculations
    - Traffic
    - Storage (5yrs)
    - Bandwidth
    - Cache (80-20 rule)


### API
  - Methods, parameters, return


### Database 
*Paradigm*
- [ ] Relational
- [ ] Key-Value
- [ ] Document
- [ ] Wide-Column
- [ ] Graph
- [ ] Search Engine


*Partitioning*
- [ ] Range
- [ ] Hash
- [ ] List
- [ ] Round-robin
- [ ] Composite

*CAP (only 2)*
- [ ] Consistancy
- [ ] Avaliability
- [ ] Partition Tolerance

*other*
- Communication Protocols
- Replication and redundancy
- Ceanup


### Cache
Local or CDN:
Required space: 

*Invalidation*
- [ ] write-through
- [ ] write-around
- [ ] write-back

*Eviction Policy*
- [ ] First In First Out (FIFO)
- [ ] Last In First Out (LIFO)
- [ ] Least Recently Used (LRU)
- [ ] Most Recently Used (MRU)
- [ ] Least Frequently Used (LFU)
- [ ] Random Replacement (RR)



### Load Balancer
*Location*
- [ ] Client  - LB - App
- [ ] App     - LB - DB
- [ ] DB      - LB - Cache

*Scheduling*
- [ ] Where to place
- [ ] Scheduling Policy
- [ ] Least Connections 
- [ ] Least Response Time
- [ ] Least Bandwidth
- [ ] Round Robin
- [ ] Weighted Round Robin
- [ ] IP Hash


### Telemetry
 

### Security and Permissions


### Design Components
  - Application Layer
  - Datastore Layer



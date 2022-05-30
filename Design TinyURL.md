---
tags: [Systems Design]
title: Design TinyURL
created: '2021-05-06T01:28:21.487Z'
modified: '2021-05-07T02:32:51.276Z'
---

**Design TinyURL**

-------------------------------------------------------------------------------------------------

# System Requierments and Goal
  - Functional Requirements:
    1. every given a URL generate a unique TinyURL even if the given URL doesnt change
    2. tinyURL should lead users to the OG URL
    3. users have the option to created a custom TinyURL
    4. tinyURL will expire at a user defined time
  - Non-Functional Requirements: avaliable, realtime
  - Extended Requirements: basic analytics


# Estimations
  - Assumptions
    - 100:1 read-to-write
    - 500M new URLS per month
    - URL object = 500 bytes
  - Calculations
    - Traffic
      - 500M URLs/month -> 100 * 500 = 50B reads/month = ~20K reads/sec
      - 500M / (30 days * 24 hrs * 3600 secs) = ~200 writes/sec
    - Storage for 5yrs
      - 500M new requests * 5 years * 12 months = 30B objects
      - 30B objects * 500 bytes = 15TB
    - Bandwidth
      - 200 writes/sec * 500 bytes = 100KB/sec
      - 20K reads/sec * 500 bytes = ~10MB/s
    - Memory (80-20 rule)
      - 20K reads/sec = ~1.7B/day
      - 0.2 * 1.7B * 500 bytes = ~170GB/day
        * since not every request would be unique, total will be less
      

# API
  - createURL(api_dev_key, original_url, custom_alias=None, user_name=None, expire_date=None): URL
  - deleteURL(api_dev_key, url_key): String


# Database Design
  - Two tables: users, URLS
  - There is no relationship between users and urls
  - noSQL DB would be a good option


# Data Partitioning, Replication, and cleanup
  - We can perform cleanup services to purge the db of expired urls. This can run whenever traffic is low. Think through what happens when users try to visit an expired URL, default expiration times, and potential storage length before cleanup occurs. 
  - For partitioning we can use either range or hash based.

# Cache
  - we know we need 170GB
  - memcached works 
  - eviction policy: least recently used (LRU)
  - each cache replica will be updated when ever there is a cache miss

# Load Balancer
  - Client (LB) App Servers (LB) DB Servers (LB) Cache Servers
  - Round Robin is simple enough to imlement but wont take server load into consideration

# Telemetry
  1. Country, time, web page, platform, etc.


# Security and Permissions
  Can users create private URLs or allow a particular set of users to access a URL?
  Since we are using a noSQL solution, we can store the user permissions with the URL object.
  We can utilize the same key for lookup. Alternatively we can make a dedicated relational table for users. 


# Basic System Design
  We want to produce a unique tinyURL for each URL such that the same URL will always output a new tinyURL. To guarentee this simply encoding or hashing incoming URLs will not be enough. We can implement a custom key generator instead. This will prevent possible duplicates and collisions. 
  
  - What should the output key be in size?
    - if each character is 1 byte in size...
    - 6 characters of a base64 encoded string will produce 68.7B unique keys = 412GB
  - How will the key generaqting service ensure unique keys and timely service?
    - for performance the keys will be generated ahead of time 
    - for redundancy we'll store keys in two DBs; have a second DB on standby
    - we can cache a few keys ahead of time to reduce latency
      - a system failure can result in losing the keys in cache 
        - but since we have a large volume of potential keys we can afford to lose them
    - What restrictions should we consider for the custom alias option?
    - How to prevent user abuse 

  - General Flow
    1. User invokes API to request a tinyURL
    2. The app server will check if OG url is mapped to tinyURL in DB, if so return URL
    3. Else the Key Service will provide a new key and this will be stored in the app DB.
    4. The Key Service will have its own db to store precomputed key.

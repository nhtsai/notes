---
layout: post
title: "System Design Overview"
description: "Notes on System Design."
toc: true
comments: false
hide: false
search_exclude: false
show_tags: true
categories: [database-systems, distributed-systems]
permalink: /system-design-overview
---

# System Design

## Scalability
* [Scalability for Dummies](https://www.lecloud.net/tagged/scalability/chrono)
* [A Word on Scalability](https://www.allthingsdistributed.com/2006/03/a_word_on_scalability.html)


### Clones
* Load balancers evenly distribute user requests to public web servers for processing.
    * Every server contains exactly the same codebase and *does not store any user-related data*, like sessions or profile pictures, on local disc or memory.
* Sessions need to stored in a centralized data store that is accessible by all application servers.
    * This way the user session can be maintained if a different application server handles the user's requests.
    * E.g. Keeps a user's shopping cart data the same if multiple servers handle the same user
* The data store can be an external database or an external persistent cache (e.g. Redis), which will have better performance than an external database.
    * **External** means data store is somewhere in or near the data center of application servers, does not reside on application servers themselves.
* **Deployment** ensures that a code change is sent to all servers without an outdated server still serving old code.
* **Clones** are instances of an machine image based upon a "super-clone" that is created from one of your servers.
    * Just do an initial deployment of your latest code to a new clone and everything is ready.

### Databases
* Using cloning, you can now horizontally scale across multiple servers to handle lots of requests.
* But one day, your application slows and breaks due to the MySQL database.
* Approach 1: Keep MySQL
    * Apply active-passive replication strategy on the database.
    * Upgrade server components like RAM.
    * Consider sharding, denormalization, SQL tuning, etc.
    * Eventually the upkeep will become too expensive.
* Approach 2: Transition to NoSQL
    * Denormalize right from the beginning.
    * Remove joins from any database query.
    * Use MySQL as a NoSQL database or switch to a NoSQL database like MongoDB.
    * If database requests still get too slow, consider a cache.

### Cache
* An in-memory **cache** (e.g. Memcached, Redis) is a simple key-value store that resides between the application and data storage.
* The application should try to read from the cache first before hitting the database because the cache is lightning fast.
    * The cache holds every dataset in *memory* (RAM) and handles requests as fast as possible, compared to *disk* data storage where I/O of reading/writing data is much slower.
* Approach 1: Cached Database Queries
    * Whenever a query on the database is run, store the result dataset in a cache.
    * Use a hashed version of query as cache key.
    * Expiration is a large issue.
        * How do we decide what cached results to evict from the cache?
        * When data is updated, all cached results calculated from that data are outdated and need to be deleted.
* Approach 2: Cached Objects
    * See the data as an object or class.
        * Let the class assemble the dataset from the database.
        * Store the complete instance of the class or assembled dataset into the cache.
    * Rather than storing results of multiple queries, we can *aggregate the results as data for a class instance* and store the instance in the cache.
    * If this ID is not present in the cache, load the data from DB, translate it and return to the client.
        * You can cache this result by translating this data into the raw data your cache has, and put it into the cache.
        * This makes it easy to delete the object when a piece of data changes.
        * This makes asynchronous processing possible.
            * Servers query the database to assemble the data in the class.
            * The application just serves the latest cached object and never touches the database.
    * *Example*: Let objects be user sessions, fully rendered blog articles, activity streams, user-friend relationships
        * The blog object has multiple methods that query the database for data.
        * Instead of caching the result of these separate database calls, cache the entire blog object.
        * When something changes, the blog object queries the database for updated data.
        * The application only has to serve the latest cached blog post instead of querying the database.

### Asynchronism
* Asynchronously doing work in advance and serving finished work with low request time.
    * Used to turn dynamic content into static content.
    * *Example*: Pre-rendering pages into static HTML files to be served quicker.
        * The rendering can be scripted to run every hour by a cronjob.
        * This pre-computing process helps make web applications scalable and performant.
* Referring unforeseen, immediate requests to an asynchronous service.
    * Upon receiving a compute-intensive task, the job is added to the job queue.
    * The job is then sent to an asynchronous server to be processed in the background.
    * Other less compute-intensive tasks can still be processed in the meantime, avoiding idle time.
    * The results of the compute-intensive job are returned once the server is done processing.
* Basic idea: Have a queue of tasks or jobs that a worker can process.
    * Backends become scalable and frontends become responsive.
* Tools to implement async processing: [RabbitMQ](https://www.rabbitmq.com/)

## Performance vs Scalability
* A service is **scalable** if it results in increased **performance** in a manner *proportional to resources added*.
    * E.g. adding another server to handle requests speeds up the website response time
* If the system is slow for a single user, there is a **performance** problem.
* If the system is fast for a single user but slow under heavy load, there is a **scalability** problem.

* An always-on service is **scalable** if adding resources to facilitate **redundancy** does not result in loss of **performance**.
    * E.g. Adding multiple replications of a database for redundancy does not decrease the query response time

* Scalability requires applications be **designed** with scaling in mind.
* Scalability also has to handle **heterogeneity**, in which servers may not work in the same manner.
    * As new resources (hardware, software) come online, some nodes will be able to process faster or store more data than other nodes in a system.

## Latency vs Throughput
* **Latency** is the time to perform some action or to produce some result.
* **Throughput** is the number of such actions or results per unit of time.
* Generally, you should aim for **maximal throughput** with **acceptable latency**.

## Availability vs Consistency
### CAP Theorem
* In a distributed system, you can only support 2 guarantees:
    * **Consistency**: every read receives most recent write or an error
    * **Availability**: every request receives a (non-error) response, without guarantee that it contains the most recent write
    * **Partition Tolerance**: system continues to operate despite arbitrary partitioning due to network failures, e.g. a server crashes or goes offline, requests are dropped, etc.
* CA or CP, or AP?
    * Networks aren't reliable, so *partition tolerance (P) needs to be supported*.
    * When a network partition failure happens, need to either:
        * Cancel the operation, decreasing availability but ensuring consistency
        * Proceed with operation, decreasing consistency but ensuring availability
    * You need to make a *software tradeoff between consistency (C) and availability (A)*.
        * Consistency (C) and availability (A) is not a practical option.
        * For (CP), system will return up-to-date response, but it may return an error or time out if a network failure occurs
        * For (AP), system will return a response if a network failure occurs, but the response may be outdated.
* For systems with no partitions or no network partition failures, all 3 guarantees can be satisfied.

### Consistency and Partition Tolerance (CP)
* System is consistent across servers and can handle network failures, but responses to requests are not always available.
* Waiting for a response from the partitioned node might result in a timeout error.
* Good choice if **atomic reads and writes** are required, e.g. banks.
    * **Atomic** refers to performing operations one at a time in a complete manner.
    * By returning an error, an incomplete operation is stopped.
* *Example*: A read operation is performed, but data propagation to all nodes is interrupted by system failure.
    * The system will 

### Availability and Partition Tolerance (AP)
* System is always available and can handle network failures, but the data is not always consistent or up to date across nodes.
* Responses return the most readily available version of the data on any node, which might be outdated.
* Writes might take some time to propagate when the partition/failure is resolved.
* Good choice if **eventual consistency** is needed or when the system needs to continue working despite external errors.

## Consistency Patterns
* With multiple copies of the same data (**redundancy**), how do we synchronize them across nodes (**consistency**) to provide all users the same view of the data?
    * The **CAP Theorem** need to respond to every read with the most recent write or an error to be **consistent**.

### Weak Consistency
* After a write, reads *may or may not* see it, and a best effort approach is taken.
* Weak consistency works well for real-time use cases, such as VoIP, video chat, and realtime multiplayer video games.
    * If you briefly lose reception during a phone call, you don't really care or hear what was lost during connection loss.
* Weak consistency is used in systems like memcached, where the result might or might not be there.

### Eventual Consistency
* After a write, reads *will eventually* see it, typically within milliseconds.
* Data is **replicated asynchronously**.
* Eventual consistency works well in highly available systems.
* Eventual consistency is used in systems like DNS and email.

### Strong Consistency
* After a write, reads *will* see it.
* Data is **replicated synchronously**.
* Strong Consistency works well in systems that need transactions.
* Strong consistency is used in systems like file systems and relational database management systems (RDBMSes).

### Transactions
* An extended form of consistency across multiple operations.
* E.g. transferring money from account A to account B
    * Operation 1: subtract from A
    * Operation 2: add to B
    * What if something happens in between operations?
        * E.g. Another transaction A or B, machine crashes
    * You want some kind of guarantee that the invariants will be maintained.
        * Money subtracted from A will go back to A.
        * Money created will eventually be added to B.
* Transactions are useful because...
    * Correctness
    * Consistency
    * Enforce invariants
    * ACID: atomic, consistent, isolated, durable

## Availability Patterns
### Fail-Over
* Active-Passive
    * Heartbeats are sent between the active server and the passive server on standby. Only the active server handles traffic.
    * If a heartbeat is interrupted, the passive server takes over the active server's IP address and resumes service to maintain availability.
    * Downtime duration is determined by whether passive server is already running in 'hot' standby or starting from 'cold' standby. 
* Active-Active
    * Both servers are managing traffic, spreading load between them
    * If servers are public-facing, DNS needs to know about public IPs of both servers.
    * If servers are private-facing, application logic needs to know about both servers.
* Disadvantages
    * More hardware and additional complexity
    * Potential for loss of data if active system fails before any newly written data can be replicated to the passive

### Replication
* Master-Slave
    * One master node handles all writes, which are then replicated onto multiple slave nodes.
* Master-Master
    * Both master nodes handle all write requests, spreading load between them. The changes are then replicated onto multiple slave nodes.

### Availability in Numbers
* **Uptime** or **downtime** is the percentage of time the service is available/not available.
* Availability is generally measured in 9s, by which a service with 99.99% availability is described as having "four 9s".

    | Acceptable Downtime Duration | 99.9% Availability | 99.99% Availability |
    | -------- | ---: | ---: |
    | Downtime per year  | 8h 45m 57.0s | 52m 35.7s |
    | Downtime per month | 43m 49.7s | 4m 23.0s |
    | Downtime per week  | 10m 04.8s | 1m 05.0s |
    | Downtime per day   | 1m 26.4s | 08.6s |

### Availability in Sequence
* Overall availability *decreases* when two components with < 100% availability are in **sequence**.
* $$\text{Availability}(\text{Total}) = \text{Availability}(\text{Foo}) * \text{Availability}(\text{Bar})$$
* If both Foo and Bar have 99.9% availability each, their total availability in sequence would be 99.8%.

### Availability in Parallel
* Overall availability *increases* when two components with < 100% availability are in **parallel**.
* $$\text{Availability}(\text{Total}) = 1 - (1 - \text{Availability}(\text{Foo})) * (1 - \text{Availability}(\text{Bar}))$$
* If both Foo and Bar have 99.9% availability each, their total availability in parallel would be 99.9999%.

## Domain Name System (DNS)
* A **Domain Name System (DNS)** translates a domain name, e.g. `www.example.com`, to an IP address, e.g. `8.8.8.8`.
    * DNS is hierarchical, with a few authoritative servers at the top level.
    * Your router or ISP provides information about which DNS server(s) to contact when doing a lookup.
    * Lower level DNS servers cache mappings, which could become stale due to DNS propagation delays.
    * DNS results can also be cached by your browser or OS for a certain period of time, determined by the **time to live (TTL)**.
* A **Name Server (NS) Record** specifies the DNS servers for your domain/subdomain.
* A **Mail Exchange (MX) Record** specifies the mail servers for accepting messages.
* An **Address (A) Record** points a name to an IP address.
* A **Canonical Name (CNAME)** points a name to another name or to an A Record, e.g. pointing `example.com` to `www.example.com`.
* Services that provide managed DNS services include: CloudFlare, Route 53, etc.

### DNS Traffic Routing Methods
* **Round Robin**
    * Pairs an incoming request to a specific machine by circling through a list of servers capable of handling the request
    * May not result in a perfectly-balanced load distribution

* **Weighted Round Robin**
    * Each server machine is assigned a performance value, or weight, relative to the other servers in the pool, usually in an automated benchmark testing.
        * This weight determines how many more or fewer requests are sent to that server, compared to other servers in the pool.
    * The result is a more even or equal load distribution.
        * Prevents traffic from going to servers under maintenance.
        * Weights can help load balance between varying cluster sizes.
        * A/B Testing

* **Latency-Based**
    * Create latency records between servers in multiple regions.
    * When a request arrives, the DNS queries the NS, which looks at the most recent latency data.
    * The load balancer with the lowest latency is the one chosen to serve the user.

* **Geolocation-Based**:
    * Choosing servers to serve traffic based on the geographic location of users.
        * E.g. routing all European traffic to a European load balancer
    * Can localize content and restrict content distribution based on region.
    * Can load balance predictably so each user location is consistently routed to the same endpoint.

### Disadvantages of DNS
* Accessing a DNS server introduces a slight delay, which can be mitigated by caching.
* DNS server management is complex and generally managed by governments, ISPs, and large companies.
* DNS services are susceptible to **Distributed Denial of Service (DDoS)** attacks, which prevent users from accessing websites without knowing Twitter's IP address(es).

## Content Delivery Network (CDN)
* A **Content Delivery Network (CDN)** is a globally distributed network of proxy servers, serving content from locations closer to the user.
    * Generally, static files (e.g. HTML, CSS, JS, photos, videos) are served from CDNs.
        * Some CDNs like AWS CloudFront supports serving dynamic content.
    * The website's DNS resolution tells clients which CDN server to contact.
* Advantages
    * Improved performance because users receive content from data centers close to them.
    * Reduced load because your servers do not have to serve requests that the CDN fulfills.

### Push CDNs
* Receive new content whenever changes occur on your server.
    * Content is only uploaded when it is new or changed, minimizing traffic but maximizing storage.
    * You take full responsibility for providing content, uploading directly to the CDN, and rewriting URLs to point to the CDN.
* You can configure when content expires and when it is updated using TTLs.
* Push CDNs work well for sites with small amounts of traffic or content that isn't often updated.
    * Content is pushed to the CDNs when needed, instead of being re-pulled at regular intervals.
    * For lots of updates, pushing content to the Push CDN places load on the server.
    * For heavy traffic, the Push CDN's cached content may not be sufficient and will place more load on the server to push content to the Push CDN.

### Pull CDNs
* Grabs new content from your server when the first user requests the content.
    * You take full responsibility for providing content and rewriting URLs to point to the CDN.
    * This results in slower requests until content is cached on the CDN, as users need to pull from the server upon the first request.
* A **time to live (TTL)** determines the life of the cached content, which you do not typically have control of.
* Pull CDNs minimizing storage space on the CDN, but can create redundant traffic if files expire and are pulled before they have actually changed.
* Pull CDNs work well for sites with heavy traffic because the traffic spread out more evenly with only recently-requested content remaining on the CDN.
    * Older requested content is expired by the TTL, making space for new content.
    * For lots of updates, the Pull CDN is able to pull and cache the updated content when requested or old content is expired.
    * For heavy traffic, the Pull CDN can serve the most requested, cached content, only pulling from the server when for less requested content.

### Disadvantages of CDNs
* CDN costs could be significant depending on traffic, although this should be compared against additional costs of not using a CDN.
* Cached content might be stale if it is updated before the TTL expires it to be updated.
* CDNs require changing URLs for static content to point to the CDN, e.g. directing `facebook.com` to `cdn-images.fb.com`.

## Load Balancer
* 


## Reverse Proxy
* Advantages
    * 
    * 
* Disadvantages

## Load Balancer vs Reverse Proxy

## Application Layer
### Microservices

### Service Discovery

### Disadvantages

## Database

* **SQL/Relational Database Management System (RDBMS)**
    * **ACID**
    * **Master-Slave Replication**
    * **Master-Master Replication**
    * **Disadvantages of Replication**

    * **Federation**

    * **Sharding**

    * **Denormalization**

    * **SQL Tuning**

* **NoSQL**
    * **BASE**
    * **Key-Value Store**
    * **Document Store**
    * **Wide-Column Store**
    * **Graph Database**

* **SQL vs NoSQL**

## Cache

* **Client Caching**

* **CDN Caching**

* **Web Server Caching**

* **Database Caching**
    * **Query Caching**
    * **Object Caching**

* **Application Caching**

* **Updating Cache**
    * **Cache-Aside**
    * **Write-Through**
    * **Write-Behind/Write-Back**
    * **Refresh-Ahead**

* **Cache Disadvantages**

## Asynchronism
* **Message Queues**
* **Task Queues**
* **Back Pressure**
* **Aynchronism Disadvantages**

## Communication
* **Hypertext Transfer Protocol (HTTP)**
* **Transmission Control Protocol (TCP)**
* **User Datagram Protocol (UDP)**
* **Remote Procedure Call (RPC)**
* **Representational State Transfer (REST)**
* **RPC vs REST Calls**

## Security



## References
[^1]: Footnote
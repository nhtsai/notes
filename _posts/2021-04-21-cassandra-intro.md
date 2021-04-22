---
layout: post
title: "Introduction to Apache Cassandra"
description: "Coursenotes from DataStax Academy DS101."
author: "Nathan Tsai"
toc: true
comments: false
hide: false
search_exclude: true
show_tags: false
categories: [cassandra, databases]
permalink: /cassandra-intro
---

# Introduction to Apache Cassandra

## Video Lecture from Patrick McFadin

[![Introduction to Apache Cassandra Video](https://img.youtube.com/vi/d7o6a75sfY0/0.jpg)](https://www.youtube.com/watch?v=d7o6a75sfY0 "Introduction to Apache Cassandra Video")

## Relational Overview

### Can RDBMS work for big data?
* Replication makes ACID a lie
    * Given a read-heavy workload, we want to create a replicated slave node from the master node. However, data is replicated asynchronously, which introduces *replication lag*. This means that if the client reads from the replicated slave node, old data will be returned and results are **not consistent** (ACID fails).
* Third Normal Form doesn't scale
    * Unpredictable queries
* Sharding is a nightmare
    * Data is split all over the place, no more joins or aggregations.
    * Querying second indexes requires hitting every shard (non-performant), so we are forced to denormalize all things.
    * Keeping shards consistent, adding shards, or changing schemas requires custom scripts.
* Low Availability
    * Who is responsible for Master failovers?
        * Whether manual or automatic, failovers need to be detected.
    * Multiple datacenter solutions are messy.
    * Downtime is frequent.

### Failure of RDBMS
* Scaling is a mess
* ACID naive at best (not consistent).
* Re-sharding is a manual process.
* Denormalize all 3rd normal form queries for performance.
* High availability is complicated to achieve, requires additional operational overhead.

### Lessons Learned for a New Solution
* Consistency is not practical --> give up consistency.
* Manual sharding and rebalancing is hard --> build it into the cluster.
* Every moving part makes systems more complex --> simplify the architecture, no more master-slave.
* Scaling up is expensive --> only use commodity hardware, more affordable.
* Scatter/Gather queries are not good --> denormalize for real-time query performance, always hit 1 machine.

## Cassandra Overview

### What is Apache Cassandra?
* Fast Distributed Database:
* High Availability: able to be accessed at all times
* Linear Scalability: linear scale-up in performance
* Predictable Performance: can guarantee [service-level agreements (SLA)](https://en.wikipedia.org/wiki/Service-level_agreement) with very low latency
* No [single point of failure (SPOF)](https://en.wikipedia.org/wiki/Single_point_of_failure): can withstand multiple points of failure
* Multi-Datacenter: can be utilized across multiple datacenters out of the box
* Commodity Hardware: can be implemented on affordable hardware when vertically scaling
* Easy to manage operationally: can manage no matter the cluster size
* Not a drop-in replacement for RDBMS: applications must be designed using Cassandra

### Hash Ring Analogy
* No master/slave/replica sets
* No config servers, zookeeper
* Data is partitioned around the ring
* Data is replicated to replication factor (`RF=N`) servers
* All nodes hold data and can answer queries (both read & write)
* Location of data on ring is determined by partition key
    * Partition key is a hash of the primary key.

### CAP Tradeoffs
* [CAP Theorem](https://en.wikipedia.org/wiki/CAP_theorem) says that when a network partition failure happens, we can either:
    * Cancel the operation and thus decrease the availability but ensure consistency
    * Proceed with the operation and thus provide availability but risk inconsistency
* Latency between data centers makes consistency impractical, especially considering datacenters across the world.
* **Cassandra chooses availbility and partition tolerance over consistency.**

### Replication
* Data is replicated automatically and asynchronously.
* Choose total number of servers to replicate data to, **replication factor (RF)**, usually 3.
    * Set when creating the **keyspace**, a group of Cassandra tables, like schema.
* Data is *always* replicated to each replica.
* If a machine is down during replication, missing data writes are replayed via **hinted handoff**.

### Consistency Levels
* Consistency level is set on per query basis, can be configured for read or write.
* `ALL`: all (100%) of replicas must confirm read/write to be considered successful.
* `QUORUM`: majority (51%) of replicas must confirm read/write to be considered successful.
* `ONE`: only 1 replica needs to confirm read/write to be considered successful, allows for fast read/write successes.

### Multiple Datacenters
* Write to local datacenter (DC) and replicate asynchronously to other datacenters.
    * Local Consistency Levels: `QUORUM` --> `LOCAL QUORUM`
* Replication factor per keyspace per datacenter.
    * Can configure different RF for each datacenter.
* Datacenters can be physical or logical.
    * Can use one DC as [OLTP](https://en.wikipedia.org/wiki/Online_transaction_processing) for fast reads and one virtual DC for [OLAP](https://en.wikipedia.org/wiki/Online_analytical_processing) queries.

## Cassandra Internals and Choosing a Distribution

### Write Path
* Writes are written to any node in the cluster, which serves as the **coordinator** for the write request.
* Writes are written to a **commit log**, an append-only, immutable data structure for durability.
* Merges the write mutation to the **memtable**, an in-memory representation of the table.
* Now Cassandra can respond to client about successful write.
    * This simplicity in write path is why Cassandra is fast.
    * Every write includes a timestamp.
* When memory is full, the **memtable** is serialized into an immutable **SSTable** and flushed to disk periodically.
* New memtable is created in memory.
* Cassandra never does any updates or deletes in-place.
* Deletes are a special write case known as **tombstone**, a marker with a timestamp to say that there is no data at that location at that time.

### What is an SSTable?
* An **SSTable** is an immutable data file for **row storage**.
* Every write includes a timestamp of when it was written.
* Partition is spread across multiple SSTables.
* Same column can be in multiple SSTables.
* Merged through **compaction**, taking small SSTables and merging them into bigger ones.
    * **Last write wins** means for a row with many changes, only latest timestamp is kept so only the latest version of the row is saved.
* Deletes are written as **tombstones**.
* Easy backups since SSTables can be copied off to another server.

### Read Path
* Any server/node may be queried, and it acts as the **coordinator**.
* The coordinator then contacts nodes with the requested key in the read query.
* On each node, data is pulled from **SSTable(s)** on disk and merged using the latest timestamp, like **compaction**.
    * The disk type (SSD or hard drive) has a large impact on the speed and performance of Cassandra.
* Consistency levels under `ALL` performs read repair in the background
    * Sometimes nodes can disagree about the value of a given piece of data since Cassandra is *eventually consistent*.
    * `read_repair_chance` specifies the chance that Cassandra will update/sync information on all other replicas, default is around 10% chance of all reads.

### Open Source Distribution
* Latest, bleeding edge features, perfect for hacking.
* File [JIRAs](https://en.wikipedia.org/wiki/Jira_(software)) issues and bug reports for support.
* Support via mailing list & IRC.

### DataStax Enterprise Distribution
* Open source Apache Cassandra at its core.
* Focused on stable releases for enterprise.
* Integrated Multi-Datacenter Search (for OLTP)
* Integrated Spark or Hadoop for Analytics (for OLAP).
* Free Startup Program (< 3MM revenue, < 30M funding).
* Extended support, additional QA.
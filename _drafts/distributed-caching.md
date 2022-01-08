---
layout: post
title: "Title"
description: "Description."
author: "Nathan Tsai"
toc: true
comments: false
# image: 
hide: false
search_exclude: true
show_tags: false
# sticky_rank: 1
categories: [tag]
permalink: /template
---

# Distributed Caching
* Caching is when we save 

## When to use a Cache
* Reduce network calls: Users are requesting the same data multiple times. Using a cache can be more efficient than querying the database every time.
* Avoid repeated computations: We can compute an expensive calculation once, then save the result in a cache.
* Reduce database load: Multiple servers hitting the database can be expensive. Using caches can avoid hitting the database.

## Why not store everything in a cache?
* Cache hardware (SSDs) are much more expensive than regular storage drives.
* Storing a lot of data in the cache increases search times, which could make the cache less efficient than hitting the database directly.


## Issues
* Poor Eviction Policy
* Thrashing
* Small Cache Capacity

## Cache locations

## Dealing with Consistency
* Write-Through Cache
    * Data is written in cache & DB; I/O completion is confirmed only when data is written in both places
    * Pros:
    * Cons: Writing to the database and the cache for every update is an expensive process. A more efficient process for write-intensive applications is Write-Back Cache.
* Write-Around Cache
    * Data is written in DB only; I/O completion is confirmed when data is written in DB.
    * Pros:
    * Cons: 
* Write-Back Cache
    * Data is written in cache first; I/O completion is confirmed when data is written in cache
    * Data is written to DB asynchronously (background job) and does not block the request from being processed
    * Under this scheme, data is written to cache alone and completion is immediately confirmed to the client. The write to the permanent storage is done after specified intervals or under certain conditions. 
    * Pros: This results in low latency and high throughput for write-intensive applications.
    * Cons: However, this speed comes with the risk of data loss in case of a crash or other adverse event because the only copy of the written data is in the cache.
* Problems with Write-Through
    * If using in-memory caches, write-through cache does not work because pushing the update through one cache will not update all caches. We must use write-back to push the update to the database and delete all existing entries from each in-memory cache.
* Problems with Write-Back
    * When updating the database

## Reference
[^1]: https://www.youtube.com/watch?v=U3RkDLtS7uY
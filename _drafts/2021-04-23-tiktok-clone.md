---
layout: post
title: "Building a TikTok Clone with Javascript"
description: "Learning Astra and Stargate (Node.js) from DataStax Developers workshop."
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

# Title

## Tools
* Cassandra: NoSQL Open Source Database which is the foundation of the stack
* Astra: Fully-managed, serverless instance of Cassandra database
* Stargate: open source engine to provide APIs for Cassandra databases, creates GraphQL, REST, etc. on top of database.
* NoSQL: Cassandra is a NoSQL database. Unlike many other systems, by default it requires a schema (the Stargate Document API does not require a schema). There is a CQL query language for working with Cassandra. CQL does not handle joins, and transactions are unbelievably fast.
* Nodes: Cassandra is designed to be distributed. You decide how many nodes you need, and you can introduce them into your instance without any downtime. Each node can handle thousands of transactions per second per core, and every node can handle any query. Nodes keep each other up to date by something called "gossiping", making sure your data stays current.
* Datacenters: Cassandra can have many nodes per datacenter, and as many data centers as you like.
* Use cases: Here's a high level overview of the use cases where Cassandra really shines.

Create an Astra database
Set up credentials in terminal
`yarn install; clear`
`node setup.js`

`npx create-react-app astra-tik-tok; cd astra-tik-tok`

`cp ../.env`




Then under user organization, create a

## References
[^1]: Footnote
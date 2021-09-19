---
layout: post
title: "Introduction to Database Systems"
description: "Course notes from CMU 15-445, Fall 2019."
author: "Nathan Tsai"
toc: true
comments: false
hide: false
search_exclude: false
show_tags: true
categories: [course-notes, databases, distributed-systems]
permalink: /database-systems-intro
---

# CMU 15-445: Introduction to Database Systems, Fall 2019

## Course Resources
* Professor Andy Pavlo
* [Course Lectures](https://www.youtube.com/playlist?list=PLSE8ODhjZXjbohkNBWQs_otTrBTrjyohi)
* [Course Website](https://15445.courses.cs.cmu.edu/fall2019/)
* [Course Syllabus](https://15445.courses.cs.cmu.edu/fall2019/syllabus.html)
* Course Textbook: Database System Concepts, 6 or 7 ed. by Silberschatz, Korth & Sudarshan
* Course focuses on design and implementation of disk-oriented database management systems.
* Topics: Relational Databases, Storage, Execution, Concurrency Control, Recovery, Distributed Databases, Potpourri

## 1. Relational Model & Relational Algebra
* [**Lecture Summary**](https://15445.courses.cs.cmu.edu/fall2019/notes/01-introduction.pdf)

* **Database**: organized collection of inter-related data that models some aspect of the real-world.
    * Databases are the core component of most computer applications.
    * E.g. a digital music store database that keeps track of artists and albums needs to store artist biographies and albums those artists released.

* **Flat File** Strawman: simplest database stored as CSV files that we manage in our own code.
    * Use a separate file per entity, e.g. one file for artists and one file for albums
    * The application parses each file to read/update records.
    * E.g. `Artist(name, year, country)` and `Album(name, artist, year)`
    * If we wanted to get the year Ice Cube went solo, we could iterate through every line in the artists file and get year of row where name is Ice Cube.

* Issues with Flat Files
    * Data Integrity
        * How do we ensure that the artist is same for each album entry, i.e. no typos or artist changes name?
        * How do we ensure the album year is a valid number and not an invalid string?
        * How do we store an album with multiple artists, i.e. check if parsed artist is single or multile?
    * Implementation
        * How do you find a particular record?
        * What if we want to use a new application with the same database, i.e. connecting logic between two languages?
        * What if two threads try to write to the same file at the same time, e.g. overwriting data?
    * Durability
        * What if the machine crashes during updating a record?
        * What if we want to replicate database on multiple databases for high availability?

* **Database Management System (DBMS)**: software that allows applications to store and analyze information in a database
    * Allows definition, creation, querying, update, and administration of databases that can be used with multiple applications.
    * Handles all the complex underlying logic for applications.
    * Focus on databases on disk, but there are other databases, e.g. in-memory.

* Early DBMSs
    * Issues
        * Database applications were difficult to build and 
        maintain
        * Tight coupling between logical and physical layers
        * Need to know what queries applications needed before deploying the database
    * Early 50s, mathematician Ted Codd (Edgar F. Codd) of IBM noticed people spent lots of time rewriting database applications, i.e. adjusting APIs depending on storing data as tree or hash table.
        * Ted proposed the **relational data model** in *A Relational Model of Data for Large Shared Data Banks* in 1970.
    * The **relational data model** database abtraction to avoid maintainence.
        * Store database in simple data structures, i.e. as relations aka tables.
        * Access data through high-level language, i.e. write code describing desired result aka declarative code instead of procedural code.
            * Software has to create query plan to get desired result, similar to compilers creating machine code from high-level languages.
        * Physical storage left up to implementation, e.g.data on disk or in-memory, storing relations as trees or hash tables, etc.

* **Data Model**: collection of concepts for describing the data in a database.
    * **Schema**: description of a particular collection of data, using a given data model.
    * Relational data model is most common.
        * The "best data model" because "9 times out of 10 that's probably what you need."
        * Can be used to model anything!
    * NoSQL Systems
        * Key/Value data model
        * Graph data model
        * Document data model
        * Column-Family data model
    * Array/Matrix data model
        * used in machine learning
    * Hierarchical or Network data models are obsolete/rare.

* Relational Model
    * **Structure**: definition of relations and their contents
    * **Integrity**: ensure database's contents satisfy constraints
    * **Manipulation**: how to access and modify a database's contents
    * **Relation (Table)**: unordered set that contains relationship of attributes that represent entities
        * **N-ary relation**: a table with N columns
    * **Tuple (Record)**: set of attribute values (aka its **domain**) for an instance of an entity in the relation
        * Values are normally atomic/scalar (no arrays or strings), originally. Now, anything can be stored.
        * The special value `NULL`, aka value is unknown, is a member of every domain.
            * How `NULL` values are stored is implementation-specific.
    * **Primary Key**: attribute or set of attributes that can uniquely identify a single tuple, usually as an id field.
        * Some DBMSs automatically create an internal primary key if not defined, e.g. `SEQUENCE` in SQL 2003 or `AUTO_INCREMENT` in MySQL.
    * **Foreign Key**: specifies that an attribute from one relation has to map to a tuple in another relation, usually as an id of a tuple in another relation.
        * Can have another table with foreign keys relating Artist IDs to Album IDs to store albums with many artists.
        * DBMSs can also check IDs exists when inserting as foreign keys.

* **Data Manipulation Languages (DML)**: how to store and retrieve information from a database
    * **Procedural**: the query specifies the high-level strategy the DBMS should use to find the desired result.
        * E.g. relational algebra
    * **Non-Procedural/Declarative**: the query specifies only what data is wanted and not how to find it, so the DBMS needs to come up with a strategy.
        * E.g. relational calculus

* **Relational Algebra**: procedural language proposed by Ted Codd for querying relational data models.
    * Fundamental operations, based on set algebra, to retrieve and manipulate tuples in a relation.
    * Each operator takes one or more relations as its inputs and outputs a new relation
    * **Select** $(\sigma)$ : choose a subset of tuples from a relation that satisfies a selection predicate.
        * Predicate is a filter to only select tuples that satisfy its requirement.
        * Can combine multiple predicates using conjunctions or disjunctions
        * Syntax: $\sigma _{\text{predicate}}(R)$
        * Example: $\sigma _{\text{a\textunderscore id='a2'}}(R)$
        * Select corresponds to `WHERE` in SQL.
    * **Projection** $(\Pi)$ : generate a relation with tuples that contains only the specified attributes.
        * Can rearrange attributes' ordering
        * Can manipulate the values
        * Syntax: $\Pi _{\text{attributes}}(R)$
        * Example: $\Pi _{\text{b\textunderscore id - 100, a\textunderscore id}}(\sigma _{\text{a\textunderscore id='a2'}}(R))$
        * Projection corresponds to `SELECT` in SQL.
    * **Union** $(\cup)$ : generate a relation that contains all tuples that appear in either only one or both input relations.
        * Relations must have same attributes with same types.
        * Syntax: $R \cup S$
        * Example: $R \cup S$
        * Union corresponds to `UNION ALL` in SQL.
    * **Intersection** $(\cap)$ : generate a relation that contains only the tuples that appear in both of the input relations.
        * Relations must have same attributes with same types.
        * Syntax: $R \cap S$
        * Example: $R \cap S$
        * Intersection corresponds to `INTERSECT` in SQL.
    * **Difference** $(-)$ : generate a relation that contains only the tuples that appear in first and not the second of input relations.
        * Relations must have same attributes with same types.
        * Syntax: $R - S$
        * Example: $R - S$
        * Difference corresponds to `EXCEPT` in SQL.
    * **Product** $(\times)$ : generate a relation that contains all possible combinations of tuples from the input relations.
        * Syntax: $R \times S$
        * Example: $R \times S$
        * Product corresponds to `CROSS JOIN` in SQL.
    * **Join** $(\bowtie)$ : generate a relation that contains all tuples that are a combination of two tuples (one from each input relation) with a common value(s) for one or more attributes.
        * Syntax: $R \bowtie S$
        * Example: $R \bowtie S$
        * Join corresponds to `NATURAL JOIN` in SQL.
    * Other operators developed over the years.
        * Rename $(\rho)$
        * Assignment $(R \leftarrow S)$
        * Duplicate elimination $(\delta)$
        * Aggregation $(\gamma)$
        * Sorting $(\tau)$
        * Division $(R \div S)$ 
    * Relational algebra defines the high-level steps of how to compute a query, instead of only stating the desired result.
        * E.g. $\sigma _{\text{b\textunderscore id=102}}(R \bowtie S)$ vs. $(R \bowtie (\sigma _{\text{b\textunderscore id=102}}(S))$
        * The first query natural joins R and S, then performs a filter after.
        * The second query filters S, then natural joins the output relation to R.
        * The queries look similar, but the efficiency could differ greatly if S and R have a billion tuples and only one tuple in S has `b_id=102`. The first query joins two huge relations, but the second one can find the one tuple where `b_id=102` before joining.
    * A better approach is to *state the high-level answer that you want the DBMS to compute*.
        * E.g. Retrieve the joined tuples from R and S where `b_id` equals 102.
        * If relations change in sizes, there no need to change the application code manually.

* Queries
    * The relational model is independent of any query language implementation.
    * **SQL** is the *de facto* standard.
    * The DBMS will take the high-level query and generate the underlying execution plan to produce the desired result.
    * The DBMS will adapt and improve itself without us having to change the application.

* Conclusion
    * Databases are ubiquitous, important, and everywhere.
    * Relational algebra defines the primitives for processing queries on a relational database.
        * Will come up again in query optimization and query execution.
    * *IMPORTANT*: The original 9 of the 36 Chambers are the RZA, the GZA, Inspectah Deck, Ghostface Killah, Masta Killa, U-God, Method Man, Ol' Dirty Bastard, and Raekwon. Cappadonna was an original member of the clan but was in jail at the time, thus could not be on the 36 Chambers.


## 2. Advanced SQL

## 3. Database Storage I

## 4. Database Storage II

## 5. Buffer Pools & Memory Management

## 6. Hash Tables

## 7. Tree Indexes I

## 8. Tree Indexes II

## 9. Multi-Threaded Index Concurrency Control

## 10. Sorting & Aggregations

## 11. Join Algorithms

## 12. Query Execution I

## 13. Query Execution II

## 14. Query Planning & Optimization I

## 15. Query Planning & Optimization II

## 16. Concurrency Control Theory

## 17. Two-Phase Locking Concurrency Control

## 18. Timestamp Ordering Concurrency Control

## 19. Multi-Version Concurrency Control

## 20. Database Logging Schemes

## 21. ARIES Database Recovery

## 22. Introduction to Distributed Databases

## 23. Distributed OLTP Databases

## 24. Distributed OLAP Databases

## 25. Shasank Chavan (Oracle In-Memory Databases)

## 26. Systems Potpourri


## References
[^1]: Footnote
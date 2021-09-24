---
layout: post
title: "Database System Concepts"
description: "Book notes on database system concepts."
author: "Nathan Tsai"
toc: true
comments: false
hide: false
search_exclude: false
show_tags: true
categories: [book-notes, database-systems]
permalink: /database-system-concepts
---

# Database System Concepts by Silberschatz, Korth & Sudarshan
* [Book Website](https://www.db-book.com/db7/index.html)
* [Online Copy](https://fliphtml5.com/pyiue/xxxf/basic)
* [Student-friendly Source](https://dokumen.pub/qdownload/database-system-concepts-7nbsped-1260084507-9781260084504.html)

## 1. Introduction to Database Systems

* **Database**: collection of data that contains information relevant to an enterprise
* **Database-management system (DBMS)**: collection of interrelated data and set of programs to access those data
    * Primary goal is to provide a *convenient and efficient* way to store and retrieve database information
    * Designed to manage large bodies of information, define structures for storage, provide ways to manipulate information, ensure safety of information in failures or breaches, avoid possible anomalous results

* Database-System Applications
    * Used to manage collections of data that are highly valuable, relatively large, and accessed by multiple users and applications, often simultaneously
    * Early database applications had simple, structured data, but modern database applications can use complex data with complex relationships
    * Databases are essential to every enterprise
        * Sales: customer, product, purchase information
        * HR: employee information, salaries, payroll taxes, benefits
        * Banking: customer information, accounts, loans, transactions
        * Universities: student information, course registration, grades
    * Two modes of database usage
        * **Online transaction processing**: large number of users retrieve small amounts of data and perform small updates
        * **Data analytics**: processing of data to draw conclusions and infer rules or decision procedures to drive business decisions
            * Creating *predictive models* that take in attributes to output predictions
            * *Data Mining* combines knowledge discovery techniques from AI and statistics to implement efficient database operations

* Purpose of Database Systems
    * Created to manage commercial data through software
    * **File-processing system**: early form of database system in which files stored permanent records and used different applications to manipulate records to appropriate files
    * Issues with file-processing systems
        * Data redundancy: copies of the same data appear in multiple files
        * Data inconsistency: copies of same data may no longer agree
        * Data access: difficulty of parsing entire files or writing custom applications for retrieving data
        * Data isolation: difficulty of working with data in various files in different formats
        * Data integrity: data values must satisfy consistency constraints
        * Data atomicity: upon failure, data should be restored to consistent state that existed prior to failure, either changes occur or not at all
        * Data concurrency-access: simultaneous manipulation of data without supervision may create inconsistent or invalid data
        * Data security: data should only be accessed by appropriate users with permission

* View of Data
    * Database system provides *abstract* view of the data, hiding implementation details of how data are stored and maintained
    * Levels of **Data Abstraction**, from lowest to highest
        * **Physical level**: describes *how* the data are actually stored in complex, low-level data structures
        * **Logical level**: describes *what* data are stored in database and relationships among data in simple structures
            * **Physical data independence**: logical level abstraction, or abstracting the complex, physical-level structures as relatively simple, logical level structures
        * **View level**: describes only part of entire database, simplifying the database to only the parts users are interested in
            * Though logical level abstraction uses simpler structures, complexity comes from variety of information stored in a large database
            * System may provide many views for the same database
    * Example: Consider `Instructor(id, name dept_name, salary)`, `Department(dept_name, building, budget)`, `Course(course_id, title, dept_name, credits)`, and `Student(id, name, dept_name, total_credits)`
        * Physical level: records are stored in blocks of consecutive storage locations
            * These details are hidden to programmers but important for database administrators
        * Logical level: each record has type definitions and relations to other records
        * View level: parts of the database are presented without logical level details in applications to users, who have the correct permissions

    * Instances and Schemas
        * **Instance**: collection of information stored in database at a particular moment
        * **Schema**: overall design of the database, not changed or changed infrequently
        * **Physical Schema**: database design at the physical level, can be changed without affecting applications
        * **Logical Schema**: database design at the logical level, most important to programmers
        * **Subschema**: schemas at the view level that describe different views of the database
        * **Physical data independence**: not dependent on physical schema, no rewrites needed if physical schema changes

    * Data Models
        * **Data Model**: collection of conceptual tools for describing data, relationships, semantics, and consistency constraints
            * Describes design of database at physical, logical, and view levels
        * There are four categories of data models
        * **Relational Model**: uses a collection of tables, or **relations**, to represent both data and relationships among those data
            * Each relation contains records with fields or attributes determined by its particular type
        * **Entity-Relationship (E-R) Model**: uses a collection of basic objects, or **entities**, and **relationships** among these objects, important for database design
        * **Object-Based Data Model**: uses object-oriented concepts of encapsulation, functions/methods, and object identity to extend entity-relationship model
            * Object-Relational model combines object-oriented data model and relational data model
        * **Semi-structured Data Model**: allows data items of the same type to have different sets of attributes, often represented in **Extensible Markup Language (XML)**
        * Other obsolete data models include **network data model** and **hierarchical data model**

* Database Languages
    * **Data-Manipulation Language (DML)**: language used to access or manipulate data as organized by appropriate data model, includes retrieval, insertion, deletion, modification
        * **Procedural DML**: user specifies *what* data are needed and *how* to get those data
        * **Non-Procedural or Declarative DML**: user specifies *what* data are needed but *not how* to get those data
        * **Query**: statement requesting information retrieval, using the **query language** of a DML
    * **Data-Definition Language (DDL)**: language used to specify database schema and additional properties of the data
        * **Data Storage and Definition Language**: language used to specify storage structure and access methods used by database system
        * **Consistency Constraints**: constraints of data values stored in database, e.g. account balance must never be negative
            * Usually checked every time a record is updated
        * Consistency constraints are implemented using **integrity constraints** that can be tested with minimal overhead
        * **Domain Constraints**: attribute must be of a certain domain (data type)
        * **Referential Integrity**: value that appears in one relation for a set of attributes  also appears in another relation for the same set of attributes
            * E.g. the dept_name of a course in the Course table must exist in the dept_name column of the Department table
            * Violations of referential integrity are caused by database manipulations, usually handled by rejecting the manipulation action that caused the violation
        * **Assertions**: any condition the database must always satisfy, every manipulation of the database is tested by the system for assertion validity
        * **Authorization**: expresses differentiation of users and their access permissions on various data values
            * **Read Authorization**: allows reading but not modification
            * **Insert Authorization**: allows insertion but not modification
            * **Update Authorization**: allows modification but not deletion
            * **Delete Authorization**: allow deletion of data
            * Users can be assigned any combination of these permissions
        * DDL output is placed a data dictionary that contains **metadata**, or data about data
            * **Data dictionary**: special type of table only internally accessible by database system, consulted before reading or modifying actual data

* Relational Databases
    * **Relational Database**: based on relational model, uses collection of tables to represent both data and relationships among those data, includes DML and DDL
    * **Table/Record**: multiple columns, each with a unique column name
        * Record based model: database is structured in fixed-format records of several types
            * Each table contains records of a particular type
            * Each record type defines a fixed number of attributes
            * Columns of table correspond to attributes of record type
        * Most basic table: CSV file
            * However, unnecessarily duplicated information is a problem
    * SQL: non-procedural query language that includes DML and DDL

    ```sql
    -- DML for query
    SELECT Instructor.id, Department.dept_name
    FROM Instructor, Department
    WHERE Instructor.dept_name = Department.dept_name 
          AND Department.budget > 95000;


    -- DDL for schema
    CREATE TABLE department (
        dept_name   char (20),
        building    char (15),
        budget      numeric (12, 2)
    );
    ```

    * **Application Program**: program that interacts with databases through using *host language* with embedded SQL queries
        * To execute DML queries from host language, can either:
            * Use API to send queries to database
            * Extend host language syntax to embed DML calls within host language program, uses DML precompiler for conversion
        * **DML precompiler**: converts DML statements to normal procedure calls in the host language

* Database Design
    * **Conceptual-Design Phase**: *what* attributes database should have and *how to group* these attributes to form the various fields
        * Characterize fully the data needs of prospective database users to produce specification of user requirements
        * Choose data model, apply concepts of chosen data model to translate user requirements into a conceptual schema of the database
            * Focus on describing data and their relationships, instead of worrying about low-level, physical implementation details
            * Can form tables using either E-R model or normalization algorithms
        * Review conceptual schema satisfies data requirements without conflicts or redundant features and satisfies functional requirements of project
        * **Specification of Functional Requirements**: users describe the kinds of operations/transactions that will be performed on the data
    * **Logical-Design Phase**: map high-level conceptual schema onto implementation data model of database system that will be used
    * **Physical-Design Phase**: specify physical features, including file organization and internal storage structures

    * **Entity-Relationship Model**: collection of *entities* and their *relationships*
        * **Entity**: distinguishable objects that are described by a set of **attributes**, e.g. `Department(dept_name, building, budget)`.
            * **Entity Set**: set of all entities of the same type
        * **Relationship**: association among several entities
            * **Relationship Set**: set of all relationships of the same type
        * Overall logical structure, or *schema*, of a data base can be expressed graphically by an **Entity-Relationship (E-R) Diagram**
        * E-R diagrams are usually drawn using the **Unified Modeling Language (UML)**
            * Entity sets are *rectangular box* with entity set name header and attributes listed below
            * Relationship sets are *diamond* with relationship name connecting a pair of related entity sets
            * **Mapping Cardinalities**: a constraint that expresses the number of entities to which another entity can be associated via a relationship set
                * E.g. an instructor must be associated with only a single department, but a department can have many instructors

    * **Normalization**: process of generating a set of relation schemas to store information without unnecessary redundancy and to easily retrieve information
        * Design schemas in an appropriate *normal form* using **functional dependencies** to determine desirable forms
        * Avoids repetition of information
            * Moving repeated data across multiple rows into a single row of a different table and adding a relationship
        * Avoids inability to represent certain information
            * Use **null** values to indicate unknown or missing values if inserting rows with incomplete information.

* Database Engine
    * Components of database system: storage manager, query processor, transaction management
    * **Storage Manager**: provides interface between low-level storage and queries from application programs
        * **Authorization and Integrity Manager**: tests satisfaction of integrity constraints and checks authority of users who access the data
        * **Transaction Manager**: ensures database remains in consistent or correct state despite system failures and concurrent transaction executions proceed without conflicts, consists of concurrency-control manager and recovery manager
            * Allows developers to treat a sequence of database accesses as if they were a single unit that either happens in its entirety or not at all
            * Abstracts away lower level details of concurrent access and system failures
        * **File Manager**: manages allocation of space on disk storage and data structures used to represent information on disk
        * **Buffer Manager**: fetches data from disk storage into main memory, decides what data to cache in main memory
            * Enables database to handle data sizes that are larger than main memory size
    * Data structures implemented by storage manager
        * **Data Files**: stores database itself
        * **Data Dictionary**: stores metadata about structure of the database, its schema
        * **Indices**: provides fast access to data items
    * **Query Processor**: helps simplify and facilitate access to data, translating non-procedural queries at the logical level to an efficient sequence of operations at the physical level
        * **DDL Interpreter**: interprets DDL statements and records definitions in data dictionary
        * **DML Compiler**: translates DML statements in query language into evaluation plan of low-level instructions for query evaluation engine
            * Query can be translated into many different evaluation plans with same result
            * **Query Optimization**: chooses the lowest cost evaluation plan for the query
        * **Query Evaluation Engine**: executes low-level instructions generated by DML compiler
    * Transaction Management
        * **Atomicity**: operations that form a single logical unit of work happen in its entirety or not at all
            * E.g. funds transfer by removing from A and adding to B should either happen together or not at all
        * **Consistency**: operations should preserve correctness of values
            * E.g. the sums of balances between A and B should be preserved
        * **Durability**: changes as a result of successful operations should be persistent despite system failure
            * E.g. new balances after funds transfer should persist even if the application crashes
        * **Transaction**: collection of operations that performs a single logical function in a database application
            * A transaction is a unit of both atomicity and consistency, so it cannot violate any consistency constraints, though transaction execution can create temporary inconsistency
            * Programmer should properly define transactions that preserve consistency
            * E.g. a funds transfer should consist of debiting account A then crediting account B, both operations together form a transaction and should not be separate
        * **Recovery Manager**: ensures atomicity and durability properties
            * Atomicity is achieved if no failures and all transactions complete successfully
            * But if transaction is incomplete due to failure, it should have *no effect* on the database
            * **Failure Recovery**: detects system failures and restores database to state prior to failure
        * **Concurrency-Control Manager**: controls interaction among concurrent transactions to ensure consistency

* Database and Application Architecture

![Database Architecture Diagram]({{ site.baseurl }}/images/database-system-concepts/database_architecture.png "database architecture diagram")

* *Shared-Memory* architectures have multiple CPUs and exploit parallel processing using a common shared memory
* *Parallel databases* are designed to run on a cluster consisting of multiple machines
* *Distributed databases* allow data storage and query processing across multiple geographically separated machines
* **Two-Tier Architecture**: application in client machine and queries database system in server machine
* **Three-Tier Architecture**: client machine acts as front-end and does not contain direct database calls, communicates with **application server** to query database system
    * **Business Logic**: specifies which actions to carry out under what conditions, embedded in central application server to handle multiple client front-ends
    * *Better security and performance* than two-tier architecture

* Database Users and Administrators
    * Naive User
        * Uses predefined user interfaces, usually a forms interface where users can fill out fields of the form
        * Views *reports* generated from database
        * E.g. student registers for courses using registration website, application verifies user identity and provides form that can query the database to enroll
    * Application Programmer
        * Computer professional who writes application programs
        * Chooses from many tools to develop user interfaces
    * Sophisticated User
        * Interacts with system without writing programs
        * Forms requests using database query language or data software
        * E.g. analysts submit queries to explore data
    * **Database Administrator (DBA)**: central control over DBMS, or data and programs that access those data
        * Schema definition: defines original schema using DDL statements
        * Storage structure and access-method definition: specifies parameters for the organization of data and indices
        * Schema and physical-organization modification: changes schema and physical organization of data to reflect changing needs or improve performance
        * Granting of authorization for data access: regulates access of database by changing authorization information kept in an internal, special structure of the DBMS
        * Routine maintenance: periodic backups to remote servers, ensuring enough free disk space or upgrading disk space, monitoring performance across workload

* History of Database Systems
    * *1950s to early 1960s*
        * Magnetic tapes allowed sequential storage of data
        * Data processed by reading from the master tape and writing to another, creating a new master tape
    * *Late 1960s to early 1970s*
        * Hard disks allowed direct access to any position in data
        * Network and hierarchical data models developed
        * Codd (1970) defines relational model and non-procedural querying
    * *Late 1970s and 1980s*
        * IBM's System R prototype leads to efficient relational DBMSs IBM's SQL/DS, UC Berkeley's Ingres, and first version of Oracle
        * Relational models eventually overtake other data models
        * Research on parallel, distributed, and object-oriented databases
    * *1990s*
        * Creation of SQL language for query-intensive workloads
        * Web interfaces for databases
        * High transaction-processing rates and high reliability
    * *2000s*
        * Semi-structured, graph, and spatial databases
        * XML and JSON data-exchange formats
        * Open-source PostgreSQL and MySQL
        * Auto-admin features for automatic reconfiguration
        * Column-stores for data analytics and data mining,
        * Map-reduce parallelism programming framework
        * NoSQL systems for lightweight data management for new types of data and eventual consistency allowed for greater scalability and availability
    * *2010s*
        * NoSQL systems develop stricter consistency and higher levels of abstractions
        * Cloud services allowed outsourcing of data storage and applications, software as a service
        * Data privacy regulations introduced


## 2. Introduction to the Relational Model
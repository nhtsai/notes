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
categories: [course-notes, database-systems, distributed-systems]
permalink: /cmu-15-445
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

* **Flat File**: simplest database stored as CSV files that we manage in our own code.
    * Use a separate file per entity, e.g. one file for artists and one file for albums
    * The application parses each file to read/update records.
    * E.g. `Artist(name, year, country)` and `Album(name, artist, year)`
    * If we wanted to get the year Ice Cube went solo, we could iterate through every line in the artists file and get year of row where name is Ice Cube.

* Issues with Flat Files
    * Data Integrity
        * How do we ensure that the artist is same for each album entry, i.e. no typos or artist changes name?
        * How do we ensure the album year is a valid number and not an invalid string?
        * How do we store an album with multiple artists, i.e. check if parsed artist is single or multiple?
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
    * The **relational data model** database abstraction to avoid maintenance.
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

<br />

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

* [Lecture Summary](https://15445.courses.cs.cmu.edu/fall2019/notes/02-advancedsql.pdf)

* SQL History
    * Originally "SEQUEL" from IBM's **System R** prototype
        * "Structured English Query Language"
        * Adopted by Oracle in the 1970s
        * Not defined by Ted Codd, the creator of relational algebra, who later on developed language Alpha
    * IBM released DB2 in 1983
        * IBM's first commercial relational database using SQL
    * ANSI Standard in 1986, ISO in 1987
        * "Structured Query Language"
    * Current SQL standard is **SQL:2016** (added JSON, polymorphic tables)
        * Every new specification/standard adds new features, which are pushed by major bodies that develop own proprietary features
    * Most DBMSs at least support **SQL-92** standard
        * No one follows an SQL standard exactly

* Relational Languages
    * The goal is for the user to specify the desired answer at a high level, not how to compute it.
    * The DBMS is responsible for efficient evaluation of the query
        * Query Optimizer: reorders operations and generates query plan
    * SQL is a collection of languages
        * **Data Manipulation Language (DML)**: commands that manipulate data
        * **Data Definition Language (DDL)**: methods of creating tables and schemas
        * **Data Control Language (DCL)**: security authorizations to control data access
        * View Definition
        * Integrity & Referential Constraints
        * Transactions
    * *IMPORTANT*: SQL is based on bag algebra and relational algebra is based on set algebra
        * **List**: a collection of elements with a defined order, can have duplicates
        * **Bag**: a collection of elements with no defined order, can have duplicates
        * **Set**: a collection of elements with no defined order, cannot have duplicates
        * If we want to define order or ensure no duplicates, the DBMS needs to do extra work to provide this.
            * The database will ignore these requests, making queries more efficient.

<br />

* Example Database
    * `Student(sid, name, login, gpa)`
    * `Enrolled(sid, cid, grade)`
    * `Course(cid, name)`

* Aggregates
    * **Aggregate function**: functions that return a single value by applying a function on a bag of tuples.
        * `AVG(col)`: Return average col value
        * `MIN(col)`: Return minimum col value
        * `MAX(col)`: Return maximum col value
        * `SUM(col)`: Return sum of values in col
        * `COUNT(col)`: Return number of values for col
    * Aggregate functions can only be used in `SELECT` output clause.
        * Example: Count number of students that have an "@cs" login.

        ```sql
        -- login field doesn't matter for COUNT
        SELECT COUNT(login) AS cnt
        FROM Student WHERE login LIKE '%@cs%'

        -- can just count every row using *
        SELECT COUNT(*) AS cnt
        FROM Student WHERE login LIKE '%@cs%'

        -- or count 1 for every row
        SELECT COUNT(1) AS cnt
        FROM Student WHERE login LIKE '%@cs%'
        ```

    * Multiple aggregates
        * Example: Get number of students that have an "@cs" login and their average GPA.

        ```sql
        SELECT COUNT(sid), AVG(gpa)
        FROM Student WHERE login LIKE '%@cs%'
        ```
    
    * Distinct aggregates
        * Example: Count number of unique students that have an "@cs" login.

        ```sql
        SELECT COUNT(DISTINCT login)
        FROM Student WHERE login '%@cs%'
        ```

    * `GROUP BY`: project tuples into subsets and calculate aggregates against each subset
        * To aggregate a columns and return a non-aggregated column, we need to use `GROUP BY`.
        * Non-aggregated columns in the `SELECT` output clause must be in `GROUP BY` clause.
        * Example: Get average GPA of students enrolled in each course.

        ```sql
        SELECT AVG(s.gpa) AS avg_gpa, e.cid
        FROM Enrolled AS e, Student AS s
        WHERE e.sid = s.sid
        GROUP BY e.cid
        ```

    * `HAVING`: filters results based on aggregation computation.
        * We cannot filter aggregations in the `WHERE` clause because the aggregation is not computed yet.
        * To filter aggregations, we must use `HAVING`, which is like a `WHERE` clause for `GROUP BY`.
        * Example: Get average GPA of students enrolled in each course with > 3.9 GPA.

        ```sql
        SELECT AVG(s.gpa) AS avg_gpa, e.cid
        FROM Enrolled AS e, Student AS s
        WHERE e.sid = s.sid
        -- cannot use: WHERE e.sid = s.sid AND avg_gpa > 3.9
        GROUP BY e.cid
        HAVING avg_gpa > 3.9
        ```

        * Knowing the `HAVING` clause can allow DBMS to optimize query.
            * In a declarative language, we can optimize because we know the desired result ahead of time, compared to a procedural language, we don't.
            * Example: Get courses with 10 students or less

            ```sql
            SELECT COUNT(s.sid) AS cnt, e.cid
            FROM Enrolled AS e, Student AS s
            WHERE e.sid = s.sid
            GROUP BY e.cid
            HAVING cnt <= 10
            ```

            * While counting the number of students for a course, if more than 10 students, DBMS can stop counting early for that course.

<br />

* Strings
    * Standard says all string fields are case-sensitive and declared with only single quotes.
    * Exceptions
        * MySQL is case-insensitive and can use double quotes
            * Standard needs to match case to match: `WHERE UPPER(name) = UPPER('KaNyE')`
            * MySQL is case-insensitive and allows double quotes: `WHERE name = "KaNyE"`
        * SQLite is case-sensitive and can use double quotes
    * `LIKE`: used for string matching
        * `%` matches any substring, including empty strings
        * `_` matches any one character

        ```sql
        WHERE e.cid LIKE '15-%'
        WHERE s.login LIKE `%@c_`
        ```

    * String functions
        * Defined in standard, can be used in either output (`SELECT`) or predicates (`WHERE`, `HAVING`)

        ```sql
        -- output
        SELECT SUBSTRING(name, 0, 5)
        FROM Student

        -- predicate
        SELECT *
        FROM Student
        WHERE UPPER(s.name) LIKE 'CHRIS%'
        ```

    * String concatenation
        * Standard says to use `||` operator to concatenate 2+ strings together
        * Exceptions: MSSQL use `+`, MySQL use `CONCAT()`

<br />

* Date/Time Operations
    * Manipulate and modify `DATE`/`TIME` attributes, can be used in either output or predicates
    * Syntax and support varies wildly
    * Example: Get current time

    ```sql
    -- PostgreSQL (SQL standard)
    SELECT NOW();
    SELECT CURRENT_TIMESTAMP;

    -- MySQL
    SELECT NOW();
    SELECT CURRENT_TIMESTAMP();
    SELECT CURRENT_TIMESTAMP;

    -- SQLite
    SELECT CURRENT_TIMESTAMP;
    ```

    * Example: Extract day from a date and getting number of days since beginning of year

    ```sql
    -- PostgreSQL (SQL standard)
    SELECT EXTRACT(DAY FROM DATE('2021-09-21'));
    SELECT DATE('2021-09-21') - DATE('2021-01-01') AS days;

    -- MySQL
    SELECT EXTRACT(DAY FROM DATE('2021-09-21'));
    SELECT DATEDIFF(DATE('2021-09-21'), DATE('2021-01-01')) AS days;

    -- SQLite
    -- cannot extract, cannot subtract dates, no datediff function
    -- method: cast date strings into julian days, subtract and cast to int
    -- Why is SQLite the most widely-used database? It's free and public domain (no copyright).
    ```

    * Simple things are difficult to do.

<br />

* **Output Redirection**: store query results in another table
    * What if we want to use query results in subsequent queries?
    * We can redirect query results into a new table.
        * Table must not already be defined
        * Table will have same number of columns and same data types as input

    ```sql
    -- SQL standard
    SELECT DISTINCT cid INTO CourseIDs
    FROM Enrolled;

    -- MySQL
    CREATE TABLE CourseIds (
        SELECT DISTINCT cid FROM Enrolled
    );
    ```

    * We can redirect query results into an existing table.
        * Inner `SELECT` must generate same number of columns and data types as target table
        * Syntax and options vary on dealing with inserting duplicate tuples
            * Target table has primary key constraint must be unique and will handle insertion of duplicate primary key values differently.
    
    ```sql
    -- assuming CourseIDs table already exists
    INSERT INTO CourseIDs (
        SELECT DISTINCT cid FROM Enrolled
    );
    ```

<br />

* **Output Control**
    * `ORDER BY`: order output tuples by values in one or more of their columns
        * Use `ORDER BY <col> [ASC|DESC]`, default is ascending order
        * Can also use any complex expression, e.g. `ORDER BY 1+1`
    * Unlike `GROUP BY`, columns in `ORDER BY` clause don't need to be in `SELECT` clause
    * Example: Get IDs of students enrolled in 15-445, sorted by descending grade then ascending student ID.

    ```sql
    SELECT sid
    FROM Enrolled
    WHERE cid = '15-445'
    ORDER BY grade DESC, sid ASC
    ```

    * `LIMIT`: limit the number of tuples returned in output, can offset to return a range
        * Use `LIMIT <count> [OFFSET <count>]`
        * Use `ORDER BY` to make results consistent 
    * Example: Get 20 student ID and names, skip first 10.

    ```sql
    SELECT sid, name
    FROM Student
    LIMIT 20 OFFSET 10
    ```

<br />

* Nested Queries: queries containing other queries, difficult to optimize
    * Inner queries can appear (almost) anywhere in a query
    * Example: Get names of all students enrolled in 15-445.
        * Naive execution: nested loops, for each student in Student, loop over sids in Enrolled
        * Better execution: produce set of enrolled student IDs once, loop over sids in Student to check if in enrolled set
        * Optimized execution: inner join tables on `s.sid = e.sid`
    
    ```sql
    -- nested query using IN
    SELECT name
    FROM Student
    WHERE sid IN (
        SELECT sid
        FROM Enrolled
        WHERE cid = '15-445'
    );

    -- join
    SELECT s.name
    FROM Student AS s, Enrolled AS e
    WHERE s.sid = e.sid AND e.cid = '15-445'
    ```

    * To construct a nested query, think about what is the answer you want to produce from the outer query.
    * Then figure out what you need from the inner query.

    * `ALL`: must satisfy expression for all rows in sub-query
    * `ANY`: must satisfy expression for at least one row in sub-query
    * `IN`: equivalent to `=ANY()`
    * `EXISTS`: at least one row is returned

    ```sql
    -- nested query using ANY
    SELECT name
    FROM Student
    WHERE sid = ANY(
        SELECT sid
        FROM Enrolled
        WHERE cid = '15-445'
    );
    ```

    * Can use subqueries in `SELECT` clause
        * for every single tuple in enrolled table where cid = 15-445, do a match up in Student table where student IDs are the same
        * essentially doing a join inside SELECT clause, reversing order of evaluating tables
        * this reversal is important for optimization because it might be faster to go through Enrolled table first

    ```sql
    -- nested query in output
    SELECT (SELECT s.name FROM Student AS s WHERE s.id = e.sid) AS sname
    FROM Enrolled AS e
    WHERE e.cid = '15-445'
    ```

    * Example: find student record with highest ID that is enrolled in at least one course

    ```sql
    SELECT sid, name
    FROM Student
    WHERE sid >= ALL(
        SELECT sid
        FROM Enrolled
    )

    SELECT sid, name
    FROM Student
    WHERE sid IN (
        SELECT MAX(sid)
        FROM Enrolled
    )

    SELECT sid, name
    FROM Student
    WHERE sid IN (
        SELECT sid
        FROM Enrolled
        ORDER BY sid DESC
        LIMIT 1
    )
    ```

    * Example: find all courses with no enrolled students
        * Inner query can reference outer query, but outer cannot reference inner.

    ```sql
    SELECT *
    FROM Course
    WHERE NOT EXISTS (
        SELECT *
        FROM Enrolled
        WHERE Course.cid = Enrolled.cid
    )
    ```

<br />

* Window Functions
    * Performs a calculation across a set of tuple that relate to a single row
    * Like an aggregation, but tuples are not grouped into single output tuples
    * Basic Syntax
        * `FUNC-NAME`: aggregation functions, special functions
        * `OVER`: how to 'slice' up data, can also sort

    ```sql
    SELECT ... FUNC-NAME(...) OVER (...)
    FROM tableName
    ```

    * Special window functions
        * `ROW_NUMBER()`: number of current row
        * `RANK()`: sort order position of current row, used with `ORDER BY`

    ```sql
    SELECT *, ROW_NUMBER() OVER () AS row_num
    FROM Enrolled
    ```

    * `OVER`: specifies how to group together tuples when computing the window function
    * Use `PARTITION BY` to specify group, like `GROUP BY` clause
    * Without `PARTITION BY`, window group is the whole table

    ```sql
    SELECT cid, sid, ROW_NUMBER() OVER (PARTITION BY cid)
    FROM Enrolled
    ORDER BY cid
    ```

    * Use `ORDER BY` to sort entries in each group

    ```sql
    SELECT cid, sid, ROW_NUMBER() OVER (ORDER BY cid)
    FROM Enrolled
    ORDER BY cid
    ```

    * Example: find student with highest grade for each course
        * Window groups tuples by course ID and orders each group by letter grade

    ```sql
    SELECT *
    FROM (
        SELECT *, 
               RANK() OVER (PARTITION BY cid ORDER BY grade ASC) AS rank
        FROM Enrolled
    ) AS ranking
    WHERE ranking.rank = 1
    ```

<br />

* Common Table Expressions (CTE)
    * Provides a way to write auxiliary statements for use in a larger query
    * Like a temp table just for one query
    * Alternative to nested queries and views
    * Basic Syntax

    ```sql
    WITH cteName AS (
        SELECT 1
    )
    SELECT * FROM cteName
    ```

    * Can bind output columns to names before the `AS` keyword
        * two named columns, return 1 row with 2 values (1, 2)

    ```sql
    WITH cteName (col1, col2) AS (
        SELECT 1, 2
    )
    SELECT col1 + col2 FROM cteName
    ```

    * Example: find student with highest id that is enrolled in at least one course
        * Query references the CTE cteSource

    ```sql
    WITH cteSource (maxId) AS (
        SELECT MAX(sid) FROM Enrolled
    )
    SELECT name
    FROM Student, cteSource
    WHERE Student.sid = cteSource.maxId
    ```

    * Difference between CTE and nested queries: CTEs can use recursion!

    * Example: print sequence of numbers from 1 to 10

    ```sql
    WITH RECURSIVE cteSource (counter) AS (
        (SELECT 1)              -- base case
        UNION ALL               -- recursive call:
        (SELECT counter + 1     --   get current value + 1
        FROM cteSource
        WHERE counter < 10)     -- specified limit of recursion
    )
    SELECT * FROM cteSource
    ```

* Conclusion
    * SQL is not a dead language.
    * You should (almost) always strive to compute your answer as a single SQL statement.


## 3. Database Storage I

* [Lecture Summary](https://15445.courses.cs.cmu.edu/fall2019/notes/03-storage1.pdf)

* Disk-Oriented Architecture
    * DBMS assumes primary storage location of the database is on non-volatile disk
    * DBMS manages movement of data between *volatile* and *non-volatile* storage

* Storage Hierarchy
    * *CPU Registers > CPU Caches > DRAM > SSD > HDD > Network Storage (e.g. AWS EBS/S3)*
    * Ordered from *faster, smaller, expensive* to *slower, larger, cheaper*
    * **Volatile**
        * not persistent without power, needs power to maintain memory
        * random access: can quickly jump around randomly in memory, same performance in any order of jumping
        * byte-addressable: can read exactly 64 bits at a location
        * volatile is DRAM and above storages, called *memory*
    * **Non-Volatile**
        * persistent without power
        * sequential access: more efficient to read contiguous blocks of storage
            * Think of a HDD like a record player, where random access requires expensive movement of needle to another location
        * block-addressable: need to get a whole 4KB block or page to read the desired 64 bits at a location
        * non-volatile is SSD and below storage, called *disk*
    * Exception: **non-volatile memory** (Intel Octane)
        * Like DRAM in that sits in DIMM slot and is byte-addressable
        * Like SSD in that it is persistent without power
        * The *future* of computers!
    * Access Times

    | Access Time      | Storage          | Comparison  |
    | ----------------:|:---------------- |:-----------:|
    |           0.5 ns | L1 Cache Ref     | 0.5 sec     |
    |             7 ns | L2 Cache Ref     | 7 sec       |
    |           100 ns | DRAM             | 100 sec     |
    |       150,000 ns | SSD              | 1.7 days    |
    |    10,000,000 ns | HDD              | 16.5 weeks  |
    |   ~30,000,000 ns | Network Storage  | 11.4 months |
    | 1,000,000,000 ns | Tape Archives    | 31.7 years  |

<br />

* System Design Goals
    * Allow DBMS to manage databases that exceed the amount of memory available
    * Reading/writing to disk is *expensive*, these calls must be managed carefully to avoid large stalls and performance degradation
        *  Use concurrent queries, caching, precomputing, etc.

* Disk-Oriented DBMS
    * At the lowest layer, we have a disk that holds database files
        * **Database file** consists of a directory and **pages**, or blocks
    * In the next layer, we have the memory that holds the **buffer pool**, which manages movement of data between disk and memory
    * At the highest layer, we have the **execution engine**, which executes queries
    * Example: The execution engine needs Page 2, but it is not in memory
        * We need to use the directory to locate Page 2
        * Fetch Page 2 from disk
        * The buffer pool will bring Page 2 into memory
        * Then we return a pointer to Page 2 to the execution engine to interpret the layout of Page 2
        * The buffer pool manager ensures the page is there while the execution engine is operating on that memory

* Why not use the OS?
    * Why does the DBMS need to 'fake' more memory than available and manage memory in this way when the OS can already do this?
        * Similar to virtual memory, where there is a large address space (virtual) and a place for OS to bring in pages from disk (physical)
        * The OS is already responsible for moving the files' pages in and out of memory 
    * Under the hood, the OS uses memory mapping (**mmap**) to store contents of a file into a process' address space
        * **mmap file**: takes a file on disk and maps the files' pages into the address space of our process
        * Now we can read and write to those memory locations
        * If the file is not in memory the OS, a **page fault** occurs
        * OS needs to bring files into memory before any operations are performed
    * *Example*
        * We have a bunch of pages in a file on-disk, a 4-slot virtual memory page table, and 2-slot physical memory
        * Application wants to read Page 1
            * We look in virtual memory and see Page 1 is not backed by physical memory, get a page fault
            * Then we fetch Page 1 from disk and back it into physical memory slot
            * Then we update our virtual page table to point to Page 1 in physical memory
        * Application wants to read Page 3, and the same process occurs and Page 3 is fetched into physical memory
        * Application wants to read Page 2, but both physical memory slots are taken
            * We need to decide which page (Page 1 or Page 3) to remove, load Page 2 into physical memory, and return a pointer in the virtual memory to the fetched data in physical memory
            * While this is happening, the thread for Page 2 is *stalled* while the disk scheduler for the OS goes to fetch Page 2
                * An optimized approach is to *hand off the stalled request task to another thread* to allow non-stalled tasks to continue executing without delay
    * The OS does not understand what the DBMS wants to do; it just sees a bunch of reads and writes to pages and does not understand the high-level semantics of queries and what data queries want to read
        * By relying on the OS, virtual memory, and mmap, we don't take advantage of our knowledge of the DBMS and *give up* control of memory management to the blind OS
    * What if we allow multiple threads to access the mmap files to hide page fault stalls?
        * This works good enough for *read-only access* because stalled requests that are being fetched can be passed to other threads to keep executing non-stalled requests
        * It is complicated for *multiple writing threads* because the OS does not know that certain pages need to be flushed out to disk before other pages do
            * The OS just writes data out to disk without knowing if it is okay or not
    * Some solutions to issues of using mmap
        * **madvise**: tell OS how you expect to read certain pages (sequential or random access)
        * **mlock**: tell OS that memory ranges cannot be paged out
        * **msync**: tell OS to flush memory ranges out to disk
    * **mmap files** and relying on the OS for memory management sound good but will create performance bottlenecks and correctness issues
        * None of the major databases use mmap completely
    * DBMS (almost) always wants to control things itself and can do a better job at it
        * knows what queries want to do and the workload
        * flushing dirty pages to disk in correct order
        * specialized pre-fetching
        * buffer replacement policy
        * thread/process scheduling
        * OS is *not* your friend

<br />

* Database Storage
    * Problem 1: How the DBMS represents database in files on disk
    * Problem 2: How the DBMS manages its memory and moves data back-and-forth from disk

* File Storage
    * DBMS stores database as one or more files on disk
        * E.g. SQLite stores databases in 1 file, most other systems store in multiple files due to file-size limitations
        * OS does not know anything about the contents of these files, but the formats of these files are typically specific to the DBMS
    * Early systems in 1980s used custom file systems on raw storage, rather than formatting the hard drive to a common file format like NTFS
        * Some 'enterprise' DBMSs still support this, e.g. Oracle DB2
        * Most newer DBMSs do not do this

* **Storage Manager**: responsible for maintaining a database's files on disk
    * Some DBMS do own scheduling for reads/writes to improve spatial and temporal locality of pages, i.e. combining queries that closely-located blocks
    * A file itself is organized as collection of pages
        * storage manager tracks data read/written to pages
        * storage manager tracks available space to store new data in the pages
    * **Page**: fixed-size block of data
        * can contain anything: tuples, meta-data, indexes, log records, ...
        * most systems do no mix page types, e.g. a page with only tuples and a page with only index information
        * some systems require a page to be **self-contained**, in which all the information needed to comprehend the contents of a page must be stored within the page itself
        * *Example*: a table with 10 columns of different types
            * Consider page's metadata about the table (schema, layout) was stored in one page and all the tuples of the table stored in another
            * An issue arises if the metadata page is lost in a system failure, then the tuples page is difficult to comprehend or interpret
            * By keeping metadata in each page, we trade overhead and storage for better disaster recovery
    * Each page is given a unique identifier, generated by the DBMS
        * The DBMS uses an **indirection layer** to map page IDs to physical location in a file at some offset
        * We want to do this to move pages around (compacting disk, setting up another disk) and still know where pages are physically located because page IDs are unchanged
    * 3 notions of pages in DBMS
        * **Hardware Page** (usually 4KB): the page access level from the storage device itself, what the HDD or SSD exposes
            * The lowest level we can do atomic writes to the storage device, usually in 4KB writes
            * The level the device can guarantee a "failsafe write", or a write and flush to the disk is atomic
            * Example: if we need to write 16KB and we write 8KB, crash, and write 8KB
                * There is a "torn write" where we only see the first half but not the second half
                * The hardware can only guarantee 4KB writes at a time
        * **OS Page** (usually 4KB): the page access level when moving data from disk into memory
        * **Database Page** (512B - 4KB SQLite - 8KB PostgreSQL - 16KB MySQL): the page access level the DBMS operates on
            * Why use more than 4KB pages?
            * Internally, we use a table to map pages to locations on disk. Using large sized pages, we can reduce that page mapping table size to represent more data with fewer page ID mappings
            * Tradeoff is handling 4KB writes very carefully

<br />

* Page Storage Architecture
    * Different DBMSs manage pages in files on disk differently, how pages are organized within files
        * Heap File Organization
        * Sequential/Sorted File Organization
        * Hashing File Organization
    * At this lowest level of hierarchy, we don't care what data is stored in the pages (tuples, indexes, etc.)

* Database Heap
    * **Heap File**: unordered collection of pages where tuples are stored in random order
        * Relational model does not have any order, insertion order is not guaranteed
        * API functions: Create, Get, Write, Delete page
        * Must support iterating over all pages for scanning entire table
    * Metadata to keep track of what pages exist and which ones have free space for inserting new data
    * Representations of heap files
        * Doubly-Linked List (naive)

            ![Heap File Linked List]({{ site.baseurl }}/images/cmu15-445/heapfile_linkedlist.png "Heap File Linked List")

            * maintain a **header page** at beginning of file that stores two pointers
                * HEAD of *free page list*, pages with available space
                * HEAD of *data page list*, pages completely full
            * each page keeps track of number of free slots in itself
                * to insert new data, we look through pages in the free page list
            * to find a particular page, we iterate over the entire data page list
            * if the pages were sorted in a unified linked list, we would have faster page searching but need to iterate over all pages to find free space

        * Page Directory (optimal)

            ![Heap File Page Directory]({{ site.baseurl }}/images/cmu15-445/heapfile_pagedir.png "Heap File Page Directory")

            * maintain a **directory page** that tracks location to *data pages* in the database files, similar to a hash table
            * directory also records number of free slots per page
            * DBMS has to make sure that directory pages are in sync with the data pages 
                * the directory holds metadata about the pages themselves that need to be in sync, but this cannot be guaranteed
                * the hardware cannot guarantee (over 4KB writes) that we can write to two pages at the same time
                * *Example*: if we deleted a bunch of data in a page and freed up space, the page directory should update the number of free slots for that page
                * But if the system crashes before the page directory is updated, the DBMS may think the page is full when it is not

<br />

* Page Layout
    * Page Header: metadata about the page's contents
        * Page Size, Checksum (used to determine torn writes/crashes), DBMS Version, Transaction Visibility, Compression Information
        * Some systems require pages to be *self-contained*
    * How do we store data within a page (assuming only storing tuples)?
        * Tuple-Oriented
        * Log-Structured
    * Tuple Storage
        * How to store tuples in a page?
        * Simple Append: strawman idea
            * Header keep tracks of the number of tuples in the page
            * Append new tuples to end (determined by offset calculated from number of tuples)
            * If we delete a tuple, there is a gap where the removed tuple was.
            * If tuples are fixed length, we can just insert a new one in the gap, but we need to sequentially scan for gaps
            * But if not fixed-length, then the gap may be too big or too small
        * **Slotted Pages**: most common layout scheme

            ![Slotted Pages]({{ site.baseurl }}/images/cmu15-445/pagelayout_slottedpages.png "Slotted Pages")

            * Header keeps track of number of used slots and the offset of the starting location of the last slot used
            * Slot array maps 'slots' to the tuples' starting position offsets
            * Tuples are stored at the end of the page, and the offset of start of the tuple is written to a slot array
                * Tuples can be fixed or variable length
                * The slot array grows from the beginning to end of page
                * The tuple storage grows from end to the beginning of page
                * Page is full when two section meet or gap too small to store anything
            * Now we can *move tuples around the page without affecting upper levels*
                * Locating a record (record Id) is just page ID and slot number
                * To move a tuple, just update its slot number
                * To delete a tuple, some systems use compaction to remove gap or some systems can mark slot value as available or some systems just append and deal with the gap later

* Tuple Layout
    * **Tuple**: essentially a sequence of bytes
    * The DBMS needs to interpret the bytes into attribute types and values
    * Prefixed with *header* that contains metadata
        * visibility info (concurrency control)
        * bitmap for `NULL` values
    * Do not need to store metadata about tuple in itself because it is stored in the page or another structure
        * Need to store metadata in JSON/Schema-less databases like MongoDB because every single document can be different, so you need metadata in itself.
    * Attributes are typically stored in the order specified when the table is created
        * Not necessary in relational model, but easier for software engineering

    * Denormalized Tuple Data
        * Can physically **denormalize** (e.g. pre-join) related tuples and store them together in the same page
            * This is when data from different tables are stored within the same page
            * Technically allowed, but now have to keep track of which tuple is from which table
        * Potentially reduces amount of I/O for common workload patterns
        * Can make updates more expensive
        * Normalization: how we split up data across different tables
            * Naturally happens with foreign keys
            * Sometimes, we want embed tables inside another table to avoid joins
            * Packing tuples inside of a tuple value
        * Not a new idea
            * IBM System R did this in 1970s, abandoned in DB2
            * Several NoSQL DBMSs (CloudSpanner, MongoDB, etc.) do this without calling it physical denormalization

* Conclusion
    * Database is organized in pages.
    * Different ways to track pages.
    * Different ways to store pages.
    * Different ways to store tuples.


## 4. Database Storage II

* [Lecture Summary](https://15445.courses.cs.cmu.edu/fall2019/notes/04-storage2.pdf)

* **Log-Structured File Organization**

    ![Log-Structured File Organization]({{ site.baseurl }}/images/cmu15-445/log_structure_fileorg.png "Log-Structured File Organization")

    * Instead of storing tuples in pages (*slotted pages*), DBMS only stores **log records**, or records of how the database was modified
        * Insert, store entire tuple
        * Delete, mark tuple as deleted
        * Update, record delta of the just the modified attributes
    * Advantages
        * *Fast writes*, sequential read/write is faster than random access on disk storage
            * In slotted pages, if we want to update 10 tuples all in different pages, then we need make and write changes at 10 separate, slotted pages
            * In log records, if we want to update 10 tuples, we just sequentially append the log records to a single page and write it out to disk.
        * Rollback capability, can revert changes to tuples
    * Disadvantages
        * Reading tuples: DBMS scans the log backwards and recreates the tuple to find what it needs
            * Can build indexes to allow jumping to particular locations in the log that reference the tuple needed
            * Can periodically compact logs back into tuples 
    * Seen in: Apache HBase, Cassandra, LevelDB, RocksDB

<br />

* Data Representation
    * **Tuple**: a sequence of bytes that the DBMS interprets into attribute types and values
        * Tuple layout determined by table schema
    * For most DBMS, we represent data similar to C++ standards
        * Integer/BigInt/SmallInt/TinyInt (C/C++/IEEE-754 standard)
        * Float/Real vs. Numeric/Decimal (IEEE-754 standard vs. fixed-point decimals)
        * Varchar/Varbinary/Text/Blob (header with length, followed by data bytes)
        * Time/Date/Timestamp (32/64-bit integer of (micro)seconds since Unix epoch)

* **Variable Precision Numbers**
    * Inexact, variable-precision numeric type that uses native C/C++ types
    * Stored directly as specified by IEEE-754 standard
    * E.g. `float`, `real`, `double`
    * Typically *faster* than (fixed point) arbitrary precision numbers because CPU has instructions to operate on these types, but can have *rounding errors* because no exact way to store decimals
        * One CPU instruction to add floating point numbers
        * Already built in functionality of underlying language

    * Rounding Example
        * Looks equal on the surface, but expanding precision shows inexactness of storing floating point numbers

    ```cpp
    #include <stdio.h>
    int main(int argc, char* argv[]) {
        float x = 0.1;
        float y = 0.2;
        printf("x+y = %.20f\n", x+y); // 0.3000...1192
        printf("0.3 = %.20f\n", 0.3); // 0.2999...8850
    }
    ```

* **Fixed Precision Numbers**
    * Numeric data types with arbitrary precision and scale
    * Used when round errors and inexactness are unacceptable
    * E.g. `numeric`, `decimal`
    * Typically stored in an exact, variable-length binary representation with additional meta-data (num digits, weight of first digit, scale factor, sign, digits)
        * Like a `varchar` but not stored as a string
    * Operations with fixed precision numbers are *slower* because of the extra calculations for the exact rounding
        * Large switch statement function to add fixed precision numbers with exact rounding
        * Need to implement this in DBMS ourselves

<br />

* Storing Large Values Internally

    ![Large Value Internal Storage]({{ site.baseurl }}/images/cmu15-445/largevalue_internal.png "Large Value Internal Storage")

    * What happens when the value we want to store doesn't fit in a single page?
        * The page size is set when the DBMS is started, usually 4KB.
    * To internally store values larger than a page, use **overflow storage pages**.
        * The tuple attribute's value is now a pointer to an overflow page using a record ID (page number + offset)
        * If the value still doesn't fit the overflow page, point to another overflow page
        * When we want to get the value, we chain the overflow pages to get all the data
    * Most DBMS do not allow a tuple to exceed the size of a single page, other DBMS have overflow pages
        * Postgres: TOAST (overflow if value > 2KB)
        * MySQL: Overflow (overflow if value > 0.5 size of page)
        * SQL Server: Overflow (overflow if value > size of page)
    * We want to use overflow pages because we get all the protections of regular data
        * If we crash when writing to overflow pages, we want to recover in the same way

* Storing Large Values Externally

    ![Large Value External Storage]({{ site.baseurl }}/images/cmu15-445/largevalue_external.png "Large Value External Storage")

    * Some systems allow you to store really large value in an external file
        * Treated as `BLOB` type, binary large object, variable length binary large data
        * Oracle: `BFILE` data type
        * Microsoft: `FILESTREAM` data type
    * To externally store values larger than a page, use a file pointer
        * The tuple attribute's value is now a pointer or filepath to some external storage device where the data can be found
    * DBMS *cannot manipulate* the contents of the external file, read only.
        * No durability protections
        * No transaction protections
    * Commonly used for storing videos or images
    * *"To BLOB or Not to BLOB"* paper from Microsoft says any values <= 256KB store as overflow page, else store in external file storage
    * SQLite inventor says to store thumbnail images up to 1MB internally is faster for phones and smaller devices because the internal file is already open, rather than following a filepath to open an external.

<br />

* System Catalogs
    * DBMS stores metadata about the databases in its internal catalogs
        * Tables, columns, indexes, views
        * Users, permissions, security
        * Internal statistics (number of values, distribution of values)
        * Should not use external information that is outside of DBMS control
    * Almost every DBMS stores their catalog inside itself as another table
        * Wrap object abstraction around tuples
            * Some kind of code to access catalogs directly, otherwise we need to query a table to find its name (which we don't know unless we query it!)
        * Specialized code for "bootstrapping" catalog tables
    * Can query the DBMS internal `INFORMATION_SCHEMA` catalog to get info about the database
        * ANSI standard specifies `INFORMATION_SCHEMA`, a set of read-only views that provide info about all of the tables, views, columns, and procedures in a database
    * DBMS also have non-standard shortcuts to retrieve this information
        * Essentially converts shortcut into ANSI standard
        * *Example*: List all tables in current database.

        ```sql
        -- ANSI/SQL-92
        SELECT *
        FROM INFORMATION_SCHEMA.TABLES
        WHERE table_catalog = '<db name>';

        -- PostgreSQL
        \d;

        -- MySQL
        SHOW TABLES;

        -- SQLITE
        .tables;
        ```

        * *Example*: Get schema for the *student* table.

        ```sql
        -- ANSI/SQL-92
        SELECT *
        FROM INFORMATION_SCHEMA.TABLES
        WHERE table_name = 'student';

        -- PostgreSQL
        \d student;

        -- MySQL
        DESCRIBE student;

        -- SQLITE
        .schema student;
        ```

    * *Main Point*: Internal catalog about database schema is used in querying and other operations
        * Easiest way to implement type system is using a switch statement to handle each type for every tuple
        * More optimized way is to use just-in-time compilation on the fly

<br />

![Workload Complexity Graph]({{ site.baseurl }}/images/cmu15-445/workload_complexity.png "Workload Complexity Graph")

* Different Workloads
    * The relational model does *not* specify that we have to store all of a tuple's attributes together in a single page
        * This may *not* actually be the best layout for some workloads
    * *Example*: Wikipedia is an OLTP workload application
        * `users(userID, userName)`
        * `pages(pageID, title, latest, revID)`
        * `revisions(revID, userID, pageID, content, updated)`

* **On-line Transaction Processing (OLTP)**
    * Simple queries read/write a small amount of data that is related to a single entity in the database
    * Ingest new data from outside world and put into database system
        * Only read a small amount of data, over and over again
    * Typical database built first for new applications, websites, etc.
    * Typical of Traditional NoSQL DBMS: MongoDB, Cassandra, Redis, BigTable
        * Focused on ingesting new data, rather than analytics
    * *Example*: Amazon.com storefront
        * Operations: adding stuff to cart, making purchases, updating account information
        * Not updating a lot of data, just your cart or account information
        * Queries just modify a small part of the database
    * *Example*: Wikipedia
        * Operations: get current revision of a page, update user login time, add a revision to a page

* **On-line Analytical Processing (OLAP)**
    * Complex queries that read large portions of database spanning multiple entities
        * Typically read-only, join queries on lots of data
    * Typical of Column-store DBMS or NewSQL DBMS or Big Data: Hadoop
        * Focused on efficient analytics, fast transaction processing without giving up joins like NoSQL DBMS
    * When you already collected a lot of data from OLTP applications and want to execute workloads on that data to *analyze and extrapolate new information*
    * *Example*: Wikipedia
        * Count the number of .gov user accounts that have logged in per month

* **Hyper Transaction Analytical Processing (HTAP)**
    * When you want to ingest new data and analyze it as it comes in
    * Commonly used in decision making on-the-fly
        * Internet advertising companies adapting while users are browsing websites

<br />

* N-ary Storage Model (NSM)/Row Storage Model

    ![NSM Diagram]({{ site.baseurl }}/images/cmu15-445/nsm_diagram.png "NSM Diagram")

    * The DBMS can store tuples in different ways depending on what is better for the OLTP or OLAP workloads
        * So far, we have assumed n-ary storage model, or row storage, of tuples.
    * **N-ary Storage Model (NSM)**: DBMS stores all attributes for a single tuple continuously in a page, row store
    * Ideal for OLTP workloads where queries tend to operate only on an individual entity
        * Accessing small amounts of data relating to a single entity, e.g. user data, is better when the entity's attributes are stored together (in a row)
    * Ideal for insert-heavy workloads
        * Inserting small amounts of related data in a row is more efficient because writing a row of data into a free slot and flush out in one disk write
    * *Example*: Accessing all account information given username and password
        * User data is stored as rows in a single tuple continuously in a page
        * Query uses username and password to lookup in an index
        * Index tells what page and slot number where the tuple can be found
        * Perform one-read, one-seek to bring the page into memory
        * Jump to location in page where tuple can be found and return result
    * Where is row-storage a bad idea?

        ![NSM Example]({{ site.baseurl }}/images/cmu15-445/nsm_example.png "NSM Example")


        * *Example*: Count the number of .gov user accounts that have logged in per month
            * Need to sequentially scan whole user account table, looking for .gov hostnames
            * Want to first look at `hostname` attribute of all users, going through each row of each page
            * Want to group accounts by `lastLogin` month of all users
            * Because disk is *block-addressable*, we can't just bring in the two columns we need; we need to bring the entire page to memory
                * Need to bring into memory all user attributes (`userID`, `username`, `password`, `hostname`, `lastLogin`) despite only needing `hostname` and `lastLogin`, 3 columns unneeded
            * Row-storage model is inefficient when analyzing data from disk that doesn't need all of the attributes
    * Advantages
        * Fast inserts, updates, and deletes
        * Good for queries that need all of the tuple's attributes
    * Disadvantages
        * Bad for scanning large portions of the table
        * Bad for queries that only need a subset of the tuple's attributes

<br />

* Decomposition Storage Model (DSM)/Column Storage Model

    ![DSM Diagram]({{ site.baseurl }}/images/cmu15-445/dsm_diagram.png "DSM Diagram")

    * **Decomposition Storage Model (DSM)**: DBMS stores values of a single attribute for all tuples contiguously in a page, column store
    * Ideal for OLAP workloads where read-only queries perform large scans over a subset of the table's attributes
        * Only need to load into memory the pages of the attribute(s) that are needed
    * *Example*: Count the number of .gov user accounts that have logged in per month

        ![DSM Example]({{ site.baseurl }}/images/cmu15-445/dsm_example.png "DSM Example")

        * User data is stored as separate pages of attributes, so we only need to bring the `hostname` pages and `lastLogin` pages into memory
            * Instead of scanning all user pages just for two attributes
        * Go through the `hostname` pages to look for .gov hostnames
        * From the matching tuples, jump to relevant locations in the `lastLogin` pages
        * Process the data needed and return result
    * Because we are storing attributes of the same type together, we can now apply compression techniques
        * *Example*: instead of storing all temperatures, store a base temperature value and a series of deltas
        * More efficient: with every page fetch we can get more compressed tuples compared to uncompressed tuples
        * Some systems can operate on compressed tuples without needing to un-compress

    * Tuple Identification

        ![Tuple Identification]({{ site.baseurl }}/images/cmu15-445/tuple_identification.png "Tuple Identification")

        * After finding the tuples with .gov in `hostname` pages, how do we match to the tuples in `lastLogin` pages?
        * Most Common Choice: **Fixed-Length Offsets**
            * Each value in an attribute/column is a fixed-length.
                * E.g. every integer is 32-bits
            * To find the same tuple in two different pages, we take the index and multiply by the fixed-length size of the attribute to get the offset or page ID (record ID + offset)
        * Alternative Choice: **Embedded Tuple IDs**
            * Each value is stored with its tuple ID in a column
            * To find the same tuple in two different pages, we just look up the same tuple ID
            * Large storage overhead, extra 32/64-bit int ID for every value

    * Advantages
        * Reduces amount of wasted I/O, moving unneeded data into memory, because DBMS only reads the data that it needs
        * Better query processing
            * Speed depends heavily on query plan, though theoretically better/faster than row storage
        * Better data compression because values of the same type are stored together
    * Disadvantages
        * Slow for point queries, inserts, updates, and deletes because of tuple splitting/stitching
            * Need to insert value in every attribute page separately
            * Need to read from multiple attribute pages to build tuple
    * History of DSM/Column Storage Model
        * 1970s: Cantor DBMS from Swedish military internal project
        * 1980s: DSM Proposal
        * 1990s: SysbaseIQ (in-memory only, first commercial product, add-on)
        * 2000s: Vertica (from creator Postgres/Ingres), VectorWise, MonetDB
        * 2010s: Everyone (Amazon RedShift, MariaDB, Cloudera Impala, etc.)

<br />

* Conclusion
    * The storage manager is not entirely independent from the rest of the DBMS
        * Underlying representation of data (row/column/etc.) is involved in other DBMS operations
    * It is important to choose the right storage model for the target workload
        * Use row store for OLTP workloads
        * Use column store for OLAP workloads
        * Some systems offer both row st1ore and column store options
        * You can mix the two in certain cases: trimming old data from OLTP for speed and moving that into OLAP data warehouse for analytics


## 5. Buffer Pools & Memory Management

* [Lecture Summary](https://15445.courses.cs.cmu.edu/fall2019/notes/05-bufferpool.pdf)

* Database Workloads
    * **On-line Transaction Processing (OLTP)**
        * Fast operations that only read/update a small amount of data each time
    * **On-line Analytical Processing (OLAP)**
        * Complex queries that read a lot of data to compute aggregates
    * **Hyper Transaction & Analytical Processing (HTAP)**
        * OLTP + OLAP together on the same database instance

* Bifurcated Environment
    * Typical Setup
        * Multiple OLTP Data Silos in the frontend
            * E.g. MySQL, PostGres, MongoDB
        * Large OLAP Data Warehouse in the backend
            * E.g. Hadoop, Spark, GreenPlum, Vertica, RedShift, Snowflake
        * **Extract-Transform-Load (ETL)** process performed to clean and move data from front end to back end
        * Perform analytics in data warehouse
        * Push any new information to the OLTP frontend, e.g. customers also bought/viewed
    * HTAP Setup
        * Multiple HTAP Data Silos in the frontend
        * Large OLAP Data Warehouse in the backend
        * ETL process performed to clean and move data from front end to back end
        * Instead of waiting on ETL process and analytics in the backend, perform some analytics in the front end

<br />

* How does DBMS manage its memory and move data back and forth from disk?
    * Due to von Neumann architecture, DBMS cannot operate directly on disk; we must bring data from disk into memory first
    * How can we bring pages into disk and support databases that exceeds the available memory?
    * How can we minimize the slowdown from movement of data, making it appear everything is in memory?

* Database Storage
    * Spatial Control
        * Where to write pages on disk
        * Goal: keep pages used together often as physically close together as possible on disk
            * Avoid long disk seeks
    * Temporal Control
        * When to read pages into memory, and when to write them to disk
        * Goal: minimize the number of stalls from having to read data from disk into memory
    * **Buffer Pool Manager**: brings the directory into memory, looks up the page location, brings the page into memory, and decides when to evict pages to free memory or write pages out to disk

<br />

* Buffer Pool Organization
    * **Buffer Pool**: *memory* region organized as an array of fixed-size pages
        * DBMS use the OS `malloc` to get a chunk of memory to use as the buffer pool
        * **Frame**: an array entry, the regions/chunks of a buffer pool
        * When the DBMS requests a page...
            * Look if the page is already in buffer pool
            * Otherwise an exact copy of the page is placed from disk into a frame in memory

* Buffer Pool Metadata

    ![Buffer Pool Page Table]({{ site.baseurl }}/images/cmu15-445/bufferpool_pagetable.png "Buffer Pool Page Table")

    * Pages can go in any order in the frames of the buffer pool, so we need something to keep track of what page is in what frame
    * **Page Table**: keeps track of pages that are currently in the buffer pool memory, hash table that maps page IDs to frame IDs
    * DBMS also maintains additional meta-data per page
        * **Dirty Flag**: single bit flag that indicates the page has been modified it was copied from disk
            * DBMS also needs to keep track of who made the modification in a log
        * **Pin/Reference Counter**: keeps track of number of threads or queries that want the page to be kept in memory
            * Avoids evicting pages to disk before operations are completed (pin)
            * Reserves frames in the page table while the corresponding page is being read into memory (lock)

* Locks vs. Latches
    * **Locks**
        * Protects the database's *logical* contents from other transactions
        * Higher level logical primitive, e.g. tuple, table, database
        * Held for transaction duration, could be multiple queries, seconds, or hours
        * Need to be able to rollback changes for concurrency control
        * Exposed to programmers, can see locks as you run queries
    * **Latches**
        * Protects the critical sections of the DBMS's *internal* data structure from other threads
        * Low level protection primitive, e.g. protecting data structures or regions of memory
        * Held for operation duration, e.g. when updating page table, apply latch on the entry (frame to page mapping) being modified before making the change
        * Do not need to be able to rollback changes
            * Internal operations, abort if cannot apply latch or operation fails
        * In OS world, this is known as a *mutex*

*  Page Directory vs. Page Table
    * **Page Directory**: mapping from page IDs to page locations in the database files
        * All changes must be recorded on disk to allow DBMS to find on restart, must be *durable*
            * If we crash and come back, we want to know where to find the pages we have
    * **Page Table**: mapping from page IDs to a copy of of the page in buffer pool frames
        * In-memory data structure that does not need to be stored on disk
            * If we crash and come back, buffer pool is erased too, so it doesn't matter

* Allocation Policies
    * **Global Policies**
        * Make decision for all active transactions of the entire workload
        * "At this point in time, what's the right thing to do to decide what should or should not be in memory?"
    * **Local Policies**
        * Allocate frames to a specific transaction without considering the behavior of concurrent transactions
        * "What's the best thing to do to make my one transaction faster?"
        * Still need to support *sharing pages*
    * For optimizations, most systems implement a combination of both allocation policies

<br />

* Buffer Pool Optimizations
    * There are a variety of ways to tailor the buffer pool to make our specific workload more efficient, better management than the OS

* **Multiple Buffer Pools**
    * DBMS does not always have a single buffer pool for the entire system
        * E.g. Enterprise DBMSs: MySQL, IBM DB2, Oracle, SyBase, MS SQL Server, Informix
    * DBMS can actually have multiple regions of allocated memory, each with page tables that map page IDs to frames in the buffer pool
        * Multiple buffer pool instances
        * Per-database buffer pool
        * Per-page type buffer pool
    * Advantages
        * Improves locality
            * Use a local policy tailored to each buffer pool and the data it holds
        * Reduces *latch contention* for different threads accessing the buffer pool
            * As a thread looks up a page ID in the page table, it latches the page table entry so the entry does not change while the thread is accessing the corresponding frame
            * Makes sure other threads do not swap the page table entry out while thread is fetching the frame of the page
        * **Latch Contention**: multiple threads contending on the same latch while accessing the same page table
            * More buffer pools = more page tables = more threads accessing different page tables at the same time = less threads contending on the same latches = more scalability
    * *Example*: single buffer pool for each table, tables with sequential vs. random access workloads are more efficient with different caching/eviction policies
    * *Example*: separate buffer pool for indexes and tables, different access patterns and workloads need different buffer pool policies
    * Approach #1: **Object ID**
        * Embed an object identifier in record IDs
        * Maintain a mapping from object IDs to specific buffer pools
        * Ensures that pages are fetched to the same buffer pool every time
            * Keeps the page in the same buffer pool to quickly find it
            * Object ID determines which buffer pool maintains that specific record
    * Approach #2: **Hashing**
        * Hash the page ID to select which buffer pool to access
        * Take the page ID, hash it, mod by number of buffer pools

* **Pre-Fetching**
    * DBMS can pre-fetch pages based on query plan to minimize stall time of fetching data from disk
        * If a thread requests a page that's not in memory, we stall that thread until the page is fetched from disk into the buffer pool, then we return the pointer to the page in memory
        * Internally the DBMS keeps track of a cursor in disk of the page that was fetched
            * If the thread needs the next page, the cursor can tell us where to fetch the next page into memory
    * Sequential Scans
        * If the query wants to scan the entire table as in sequential read, the DBMS can *pre-fetch* all pages of the table into the buffer pool, rather than wait to fetch page by page
            * Pages already processed can be evicted and replaced with the next page
    * Index Scans
        * The index allows you to jump to specific points in the data based on values
        * If the query wants values in a specific range, the index can tell us which pages pages have values within the range
        * Because the query wants specific, non-sequential index pages, the DBMS can *pre-fetch* the specific index pages into the buffer pool, rather than sequentially fetching unneeded pages
    * Advantage
        * Pre-fetching using sequential read minimizes random I/O and reduces disk stalls

* **Scan Sharing**
    * Queries can *reuse* data retrieved from storage or operator computations
        * Multiple queries can use the same data from disk
        * Different from **result caching**: returning cached answer instead of re-computing query
    * Allow multiple queries to attach to a single cursor that scans a table
        * Queries do not have to be exactly the same
        * Can also share intermediate results across different threads in some cases, *materialized view*
            * Intermediate results are stored in a separate memory region outside of buffer pool
    * If a query starts a scan and there's an existing query doing the same scan, the DBMS will attach the new query to the existing query's cursor
        * DBMS keeps track of where new query joined with existing query so that it can finish the scan when it reaches the end of the data structure, go back to read previous data that was read before new query joined
    * Fully supported in IBM DB2 and MS SQL Server, Oracle only supports basic scan sharing (cursor sharing) for identical queries
    * *Example*
        * First query wants to compute sum on table A
        * Pages are fetched, buffer pool fills up and replaces older frames (Page 0) with new page copies (Page 3)
        * Second query wants to compute average on table A
            * It hops on to the same cursor as the first query, which is already at Page 3
        * The cursor scans the rest of the disk pages into buffer pool frames that are needed for the first query
        * After the first query is done reading data, the cursors disappears
        * Second query then starts cursor from beginning at Page 0
            * Second query then reads until Page 3 to make up for missing pages as needed
    * The relational model is unordered; answers can be correct without being the same value every time
        * *Example*: Query wants to compute average, limit 100 tuples
            * Without a shared cursor, the query will read the first 100 tuples, starting from the first disk page
            * If the query hops on to a shared cursor, the first 100 tuples will be whatever is on the disk pages that the cursor is at
            * If the data needed is already in the buffer pool, the query can use the existing data in memory
        * All methods can produce a "correct" answer without reading the same data to compute the same value because data is stored unordered

* **Buffer Pool Bypass**
    * Sequential scan operator will not store fetched pages in the buffer pool in order to avoid overhead of looking at page table
        * Sequential scan does not look at the page table to see if the page is already in memory
        * Sequential scan also does not want to pollute the cache with data that may not be needed in the near future
        * Sequential scan just keeps scanning sequential disk pages and evicting them once finished processing in memory
    * **Buffer Pool Bypass** or **Buffer Cache Bypass**
        * Allocate a small amount of memory to the query
        * The query reads pages, fetching from disk if not already in the buffer pool
        * The pages are fetched into a local memory area, instead of the buffer pool
            * Memory is local to running query and the thread running it
        * When the query is done, all of the local memory is freed at once
    * Advantages
        * Avoids overhead of going to the page table, a hash table with latches
        * Works well if operator needs to read a large sequence of pages that are contiguous on disk
        * Can also be used for smaller temporary data, intermediate tables (sorting, joins)
            * Larger temporary data needs to be backed by the buffer pool, written as pages out to disk if larger than memory
    * Most DBMS support this optimization, called light scans in Informix

<br />

* OS Page Cache
    * What is the OS actually doing at a lower level?
    * Most disk operations go through the OS API (`fopen`, `fread`, `fwrite`)
    * Unless specified otherwise, the OS maintains its own filesystem cache
        * By default, the OS maintains the **OS page cache**
        * As the DBMS reads pages from disk, the OS also caches a copy of the page
    * Most DBMS use **direct I/O** (`O_DIRECT` POSIX flag) to bypass the OS's cache
        * Avoid redundant copies of pages
        * Different eviction policies between DBMS buffer pool and OS page cache
        * Can also help maintain similar performance across different OS's
    * Postgres is the only major DBMS that relies on the OS page cache
        * Trade avoid maintaining a separate caching system for a minor performance penalty
        * Main disadvantage is there could be two copies of every single page, and the OS page cache could be old if the DBMS modifies the page
    * If we give the DBMS enough memory, it can store the whole table in memory, which will make queries on that table faster since we don't have to fetch from disk

<br />

* Buffer Replacement Policies
    * What happens if we need to fetch a page from disk but the buffer pool is full?
    * When the DBMS needs to free up a frame to make room for a new page, it must decide which page to **evict** from the buffer pool
    * Goals
        * Correctness: we don't want to write out data from latched frames before threads are done modifying
        * Accuracy: we want to make sure we evict pages that are least likely to be used in the future to minimize future disk seeks and stalls
        * Speed: we want to quickly decide which pages to evict from frames to make space for new fetches
        * Meta-data overhead: we don't want metadata for a page (e.g. how likely it's going to be used) to be larger than the page itself
    * Higher-end DBMS have complex replacement policies compared to open-source DBMS

* **Least-Recently Used (LRU)**
    * Maintain a timestamp of when each page was last accessed
    * Evict the page with the *oldest timestamp*
    * Keep pages in sorted order to reduce search time on eviction
    * *Intuition*: If the page hasn't been used in a while, it probably won't be used often in the future, and we can evict it

* **CLOCK**
    * Approximation of LRU without needing a separate timestamp per page
        * Each page has a *reference bit*, which is set to 1 if the page is accessed since the last time it was checked
    * Organize pages in a circular buffer (clock), sweeping through with a "clock hand"
        * Upon sweeping, check if a page's bit is set to 1
        * If the bit is set to 1, we know it was accessed, and reset to 0
        * If the bit is not set to 1, we know it was not accessed, and evict it
        * Evicted frames can be re-populated with new fetched disk pages

* Problems with LRU and Clock
    * **Sequential Flooding**
        * A query performs a sequential scan that reads every page
        * This pollutes the buffer pool with pages that are read once and then never again
        * The pages with the oldest timestamps that are important to keep are pushed to eviction by a sequential scan of many new pages
        * The *most recently used page* is actually the page we should evict in this case
    * *Example*
        * Query 1 wants to find tuples with ID = 1 and fetches Page 0
        * Query 2 wants to find average of all values and starts a sequential scan
        * The buffer pool fills up (Page 1, Page 2) and a page needs to be evicted
            * Under LRU replacement policy, Page 0 is the oldest and should be evicted to be replaced with Page 3
        * Query 3 wants to do the same thing as Query 1
            * Now Page 0 needs to be re-fetched even though it was just evicted!
        * The best approach would have been to evict either Page 1 or Page 2 because the sequential scan is just going to keep reading more pages, not using Page 1 or Page 2 ever again

* **LRU-K**
    * Track the history of last K references to each page as timestamps
        * List of timestamps of every time the page was accessed
    * Compute the interval between subsequent accesses
        * Which page has the largest interval, or the longest amount of time between accesses?
    * DBMS then uses this history to estimate the next time that page is going to be accessed
    * Used in more sophisticated DBMS

* **Localization**
    * DBMS chooses which pages to evict on a per transaction/query basis
        * Rather than putting sequentially scanned pages into the global buffer pool, set aside some frames in the buffer pool for a specific query
        * Evict pages least recently used for that specific query, not the global view
    * Minimizes pollution of buffer pool from each query
    * Postgres maintains a small ring buffer that is private to the query

* **Priority Hints**
    * DBMS knows the context of each page during query execution
    * DBMS can provide hints to the buffer pool on whether a page is important or not
    * *Example*
        * We continuously run queries to insert an incrementing ID value, like auto-increment
        * If the index is sorted on ID in ascending order, we know we want to keep accessing increasing index pages
        * Therefore, DBMS can tell buffer pool that previous index pages are less important and should be evicted

<br />

* **Dirty Pages**
    * How do we handle dirty pages, which have been modified since being fetched into the buffer pool?
    * *FAST*: If a page in the buffer pool is *not dirty*, then DBMS can simply "drop" it
    * *SLOW*: If a page in the buffer pool is *dirty*, DBMS must write back to disk to ensure changes are persisted
    * Tradeoff in replacement policy between fast evictions vs. dirty writing pages
        * If a non-dirty page will be used in the future, DBMS should take the penalty of writing a dirty page out to disk
        * Writing a dirty page is 2 disk I/O's: 1 to write page to disk and evict from buffer pool, 1 to fetch new page into buffer pool
        * Evicting a non-dirty page is 1 disk I/O: evict from buffer pool and 1 to fetch new page into buffer pool

* Background Writing
    * How do we figure out whether to keep a non-dirty page or a dirty page?
    * DBMS can periodically walk through page table and write dirty pages to disk
    * When a dirty page is safely written, DBMS can either evict the page or just unset the dirty flag
        * The replacement policy can now drop more clean pages, since dirty pages have already been written to disk
    * Need to be careful that dirty pages are not written to disk before their log records have been written to disk
        * Don't want to write to disk without first recording the modifications made

* Other Memory Pools
    * DBMS needs memory for things other than just tuples and indexes
    * Other memory pools may not be backed by disk, depending on implementation
        * Sorting, Join Buffers
        * Query Caches
        * Maintenance Buffers
        * Log Buffers
        * Dictionary Caches

<br />

* Conclusion
    * The DBMS can manage that sweet, sweet memory better than the OS can
    * Leverage semantics about the query plan to make better decisions
        * Evictions/Replacement Policy
        * Allocations/Allocation Policy
        * Pre-fetching/Other Optimizations


## 6. Hash Tables

* [Lecture Summary](https://15445.courses.cs.cmu.edu/fall2019/notes/06-hashtables.pdf)

* 

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
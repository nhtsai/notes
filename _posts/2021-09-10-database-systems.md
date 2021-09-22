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
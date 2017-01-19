---
layout: post
title:  "SQL anti-pattern Query anti-pattern"
date:   2016-11-05 12:00:00 +0900
categories: Development
---

Today I read the SQL anti-pattern,
so I will extract some of the terms and anti-patterns that I came out and summarize.

## ①Fear of the Unknown

An anti-pattern that uses NULL as a general value or treats a general value as NULL

### Demerit

- Since NULL is not the same as zero, concatenating a string with NULL returns NULL
- unknown will be returned when searching
- Difficult to handle NULL like general value in SQL parameterized with prepared statement

### When we can use anti-pattern

Using NULL itself is not an anti-pattern.
It is an anti-pattern that NULL is used as a general place or general value is treated as equivalent to NULL.

## ②Ambiguous group

An anti-pattern that refers to a non-grouping column

### Demerit

- Single-Value Rule
- SQL does not always make the intent of the query ... Since the MAX function is used for another column, SQL will properly judge which `bug_id` you want to output

#### Small note

Using the DISTINCT keyword for query qualifiers reduces the rows in the query result and makes all rows unique.

### When we can use anti-pattern

MySQL and SQLite can not guarantee the reliability of results against columns that violate the single value principle.
We should try to use unambiguous columns.

## ③Random selection

An anti-pattern that sorts data randomly.

### Demerit

- Sorting by non-deterministic expression (RAND function) can not get the benefit from the index
- If indexes can not be used, the table must be sorted manually by query results (table scan) index is much slower than sorting

### When we can use anti-pattern

When the data set is small

## ④Poorman's Search Engine

Anti-pattern to use pattern match terminology.

### Demerit

- Scan all rows of table because you can not get the benefits of the index
- Unintended match occurred

### When we can use anti-pattern

If you use it correctly for simple applications you can get great value.
Using pattern matching terminology for complex queries faces difficulties.

As a solution use appropriate tools such as full text search engines.

- MySQL full text index
- Text index in Oracle
- Full text search on Microsoft SQL Server
- Text search in PostgreSQL
- Full text search (FTS) in SQLite
- Third party search engines

## ⑤Spaghetti Query

Anti-pattern to solve complicated problems in one step

### Demerit

- Unintended result ... *** Cartesian product *** arises. It is created when two tables specified in a query do not have conditions to restrict association. Combining the two tables makes each row of one table pair with all the rows of the other table
- It is difficult to describe, modify, and debug queries
- Cost increase at runtime

### When we can use anti-pattern

If the report's requirements are too complicated to achieve with a single SQL query, it is better to create multiple reports.

## ⑥implicit column

Software developers do not like to hit a lot of keys.
Specifying all column names in SQL queries tends to avoid using wildcards.

An anti-pattern that falls into a shortcut trap.

### Demerit

- There is a possibility that performance and scalability may be adversely affected

### When we can use anti-pattern

It is reasonable when you want to draw ad hoc SQL quickly.
Column names should be specified explicitly whenever possible.
---
layout: post
title:  "SQL anti-pattern Anti-pattern of database physical design"
date:   2016-11-04 12:00:00 +0900
categories: Development
---

Today I read the SQL anti-pattern,
so I will extract some of the terms and anti-patterns that I came out and summarize.

## ①Rounding error

An anti-pattern to use the FLOAT data type
In order to effectively use the FLOAT type, it is necessary to understand the characteristics of the floating point number.

### Demerit

- Rounding is inevitable
- Floating-point error cumulative is even greater when calculating the product instead of the sum

### When we can use anti-pattern

- Applications that make scientific calculations

Instead of FLOAT and its similar data type, use NUMERIC or DECIMAL of the SQL data type to represent the fixed precision decimal point number.

## ②Thirty One Flavor

It is useful to limit the value that can be stored in a column to a limited value. (CHECK (salutation in ('Mr', 'Ms')))
It can be guaranteed that an invalid value is not included in the column.

An anti-pattern that specifies a restricted value in a column definition.

### Demerit

- The trouble of examining the contents of the column definition
- If you fail to maintain it will not be synchronized
- Adding new definitions should not be done frequently
- If you abolish one of the definitions, affect past data
- difficult to transplant

### When we can use anti-pattern

Adoption of ENUM is not a problem as long as the value set does not change.

A solution that specifies values to be limited with data.

## ③Phantom file

An antipattern that thinks that the use of a physical file is indispensable.

### Demerit

- Problems when deleting files ... Files that became "orphans" accumulate
- Transaction isolation problem
- Problems that can be placed in rollback
- Problems when using DB backup tool
- Problems when using SQL access authority ... Access rights assigned by SQL statements such as GRANT and REVOKE are not applied to external files.
- File is not SQL data type

### When we can use anti-pattern

Storing objects with large data sizes such as images in files external to the database.

- Reduce DB capacity
- Back up quickly
- Easy preview and editing if the image file is outside the DB

When these merits are important to the project and problems are not serious.
However, we should always consider two designs.
The BLOB type is adopted as necessary.

## ④Index shotgun

The best way to improve database performance is to use *** index *** effectively.
However, there are not many software developers who properly understand when and how to use indexes.
Based on speculation, developers can only explore ways to use indexes effectively.

### Demerit

- We tend to do not define an index at all or only define a few indexes
- We tend to define a lot of indexes Define too much or not useful index
- We tend to run a query that does not utilize indexes

### Solution

Effective index management should be done based on MENTOR principle

- Measure (measurement) ... slow query log
- Explain ... Using Query Execution Plan (QEP)
- Nominate (nominate) ... Find a place accessing the table without using the index
- Test
- Optimize ... indexes are more likely to be stored in cache memory. Setting of the amount of memory allocated to the cache.
- Rebuild (rebuild) ... regular maintenance
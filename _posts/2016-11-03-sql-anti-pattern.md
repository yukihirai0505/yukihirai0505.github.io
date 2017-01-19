---
layout: post
title:  "SQL anti-pattern Anti-pattern of database logic design"
date:   2016-11-03 12:00:00 +0900
categories: Development
---

Today I read the SQL anti-pattern,
so I will extract some of the terms and anti-patterns that I came out and summarize.

## Summary of quotes in this chapter

> Ihr seid alle Idioten zu glauben, aus Eurer Erfahrung etwas lernen zu konnen,
  ich ziehe es vor, aus den Fehlern anderer zu lernen, um eigene Fehler zu vermeiden.
  qt: http://okanos.com/2012/02/10202432.php

> An expert is a person who has made all the mistakes that can be made in a very narrow field.
  by Niels Bohr

> One in a million is next Tuesday
  by Gordon Letwin

## ①Jay Walk (signal ignored)

Developers often express "many-to-many" relationships *** to avoid creating cross-cutting tables ***
Use a comma-separated list.
We call this Jay Walk (Ignore Signal).

It is an act of trying to avoid "intersection".

### Demerit

Performance degradation and data consistency problems

- Pattern matching is necessary for some character string pattern because comparison by equivalence is not possible
- Pattern matches can return unintended results by writing expressions
- I can not get the merit of using the index
- It takes time and effort to join (JOIN) and it will not be efficient
- When using aggregate queries (COUNT, SUM, AVG), development takes time and debugging becomes difficult
- Account updates become redundant
- It is difficult to validate account ID
- It is troublesome if there is a delimiter in each input value
- Loss restricts to length

### When we can use anti-pattern

When applying *** denormalization *** to the database structure
However, sufficient consideration is necessary.
Flexibility will be higher if it is normalized and it will help to maintain data integrity.

## ②Naive tree

When you can comment on each thread at the news site.
When comments are expressed in a tree structure
Each entry is called *** node ***.
A node can have one or more children and one parent.
The top node without parent has root (root)
The lowest node without children has leaves (leaves)
The middle node is a non-leaf node

Typical example of tree-oriented data structure

- Organization chart
- Thread style comment field

The idea of adding a `parent_id` column is inconceivable, it can be said to be naive.
`Parent_id` refers to another row in the same table.

Such a design is called an adjacency list.

### Demerit

- Writing queries to get all descendants becomes redundant
- It is difficult to handle aggregate functions like COUNT
- When deleting a node, it must be deleted from the bottom layer in order (it can be automated by ON DELETE CASCADE but node promotion and movement can not be automated)

### When we can use anti-pattern

- Get nearest parent and child of the node only
- Easy insertion of columns

It is effective when the operation which is necessary for data having hierarchical structure is only such as.
There are also products with SQL extension. * Recursive query *

***solution***

Use alternative tree model

- Path Enumeration model
- Nested Set
- Closure Table model

### Path Enumeration model

Adjacency list has weak point that efficiency of obtaining ancestor node is bad.
We solve the problem by storing character strings expressing genealogies of the ancestors as attributes of each node.
Define the `path` column with a string instead of` parent_id`.

However, there is a weak point indicated by Jay Walk.
The DB can not guarantee the exact shape of the path and correspondence of the value of the path to the existing node.

### Nested Set

Information on a set of descendants, not the nearest parent, is stored in each node.
This information is expressed as a number called `nsleft` or` nsright` of each node.

nsleft ... There is a value smaller than the value of all the nodes in the hierarchy below that node
nsright ... A value larger than the value of all the nodes in the hierarchy below that node is entered

A major advantage of the nested structure design is that when a virgin leaf node is virgin, the descendant of the deleted node is automatically considered to be a direct child of the deleted node's parent.
However, in nested structures, some of the queries which can be easily executed in the adjacency list such as acquisition of the nearest parent becomes complicated.
This is useful when quick execution of queries on subtrees is important rather than operation on individual nodes.
Insertion and movement of nodes becomes complicated because recalculation of left and right values ​​of related nodes is required.

### Closure Table model

Simple and elegant storage method of hierarchical structure.
In addition to direct parent-child relationships, store the path of the entire tree.

In addition to the simple comments table, a tree_paths table is newly defined.
Tree_paths stores combinations of nodes sharing ancestor / descendant relationships.
All nodes including nodes at distant positions on the tree are targeted.

However, the query to the nearest child and parent is somewhat complicated.
But, it becomes easy to use it by adding a path_length column.

## ③ID Requirement

*** Understanding the primary key ***.
It guarantees that all the rows of the table are unique,
We deal with individual rows, the logical mechanisms necessary to avoid row duplication,
The primary key is referenced from the *** foreign key *** to associate the table.
To store an artificial value that has no meaning in that area in the table modeling a certain target area
Add a new column.
By using this column as the primary key, we allow duplication in other attribute columns, so that we can treat the line as unique.
*** Pseudo key *** or called ****  surrogate key ***.

Role of primary key

- To avoid row duplication
- To reference individual rows in a query
- To support foreign key reference

An anti pattern that uses the "id" column for all tables.

### Demerit

- A redundant key is created ... When another column in the same table is likely to be used as *** "natural" primary key (natural key) *** or when it is likely to be able to grant a UNIQUE constraint To be redundant
- This might Allow duplicate rows ... *** Compound key *** is a key composed of multiple columns (in case of a combination of bug_id and product_id primary key of BugsProducts table) is unique We must ensure that there is. However, using the id column as the primary key will not guarantee that the combination is unique
- The meaning of the key is hard to understand ... The column name id does not have a clear meaning.
- Using USING ... When using USING, you can not use the same name as the referenced primary key for the foreign key column on the dependent table side, and use redundant ON syntax.
- Compound keys are hard to use ... Although they can be simplified, they do not accurately represent real world objects to be targeted

### When we can use anti-pattern

Some object relational mapping (ORM) frameworks to simplify development
*** convention over configuration ***.
However, since you can also overwrite this convention, you should adjust it accordingly.

## ④Keyless entry

In relational database design, as important as the design of each table,
Design of relation (relationship) between tables.
*** Referential integrity *** is extremely important in proper database design and operation.

Main reason for not using referential integrity (not using foreign key)

- Data update conflicts with referential integrity constraints
- Design flexibility is extremely high so support referential integrity is not available
- Indexes created for foreign keys affect performance
- You are using a DB product that does not support foreign keys
- You have to look up the syntax declaring foreign keys

An anti-pattern that does not use foreign key constraints.

### Demerit

- It is premised for perfect code ... It is a prerequisite not to make mistakes
- You have to investigate mistakes - you can also run hundreds of checks every day, or multiple times a day
- Catch = 22 UPDATE ... Sometimes you have to change parent and child at the same time. It is impossible to execute two different update processes simultaneously. Even when you want to avoid this you can solve by using a foreign key. (Cascade update: declare ON UPDATE, ON DELETE clause for foreign key constraint)

### When we can use anti-pattern

When using DB products that do not support foreign key constraints. (MyISAM's MyISAM storage engine and SQLite less than version 3.6.19)

## ⑤EAV(Entity · Attribute · Value)

In the case of grouping by date, there are the following assumptions.

- The values are stored in the same column
- Can compare values

It is not easy to compare if the date format is different or data is stored in a different column.

An anti-pattern (ex: attr_name, attr_value) to use a general-purpose attribute table

### Demerit

- We can not set mandatory attribute
- We can not use data type
- We can not enforce referential integrity
- We must complement the attribute name
- We have to rebuild the line

### When we can use anti-pattern

Because many of the advantages of relational databases are lost, there is little reason to justify EAV.
*** Nonrelational ** If you need data management, you should use nonrelational techniques.

- Berkeley DB ... DBLibrary of key / value expressions
- Cassandra ... Column-oriented distributed DB developed on Facebook
- CouchDB ... Document Oriented DB. Store distributed key / value and encode value in JSON format
- Hadoop / HBase ... An open source distributed DB that implements Google's MapReduce algorithm.
- MongoDB ... Document-oriented DB similar to CouchDB
- Redis ... Document oriented in-memory DB
- Tokyo Cabinet ... A data store of key / value type that supports POSIX DBM, GNU GDBM, Berkeley DB and so on.

*** EAV solution ***

- Single table inheritance
- Concrete table inheritance
- class table inheritance

## ⑥Polymorphic associations

If there are two similar tables

An anti-pattern that uses a dual-purpose foreign key

- polymorphic associations
- promiscuous association

Multiple tables can be referenced.

### Demerit

- We can not define referential integrity constraint ... In this case, in addition to a foreign key such as `issue_id`, a column for association such as` issue_type` is required. In this case foreign key declaration for `issue_id` can not be done. For a foreign key, only one table has to be specified.
- It may also be necessary to execute queries with outer joins of both parent tables
- Non-object oriented

### When we can use anti-pattern

Avoid polymorphic-related use whenever possible and use foreign key constraints to guarantee referential integrity.
When using polymorphic association it relies heavily on application code, not metadata.
When using an ORM framework such as Hibernate, there are occasions when it is necessary to use this anti-pattern.
We encapsulate polymorphic-specific application logic to maintain referential integrity, thereby reducing the risk of adopting polymorphic-related applications.

## ⑦Multicolumn Attribute

A problem of how to store it if there are multiple values in an attribute that seems to belong to the same table as Jay Walk.
You want to add a tagging function to classify bugs in the bug database.

An anti-pattern that defines multiple columns.
A pattern that adds columns rather than storing clothes values in a single column separated by commas.

### Demerit

- Searching for values ... troublesome
- Adding and deleting values ... Requires complex SQL
- Uniqueness guarantee ... possibility that the same value is stored in multiple columns
- Processing of incrementing values ... Extension required according to the maximum number of tags

### When we can use anti-pattern

When you can limit the choices of attributes.

## ⑧Metadata Triple

An anti pattern that copies tables and columns

- To divide a table with many rows into multiple tables (naming the table based on distinguishable data values of a certain attribute of a table)
- To divide the column into multiple columns (naming the columns based on distinguishable values of other attributes)

### Demerit

- Propagation of the table ... You may have to create a new metadata object for the new data
- Data consistency management ... need to prevent forgetting to correct the value of the CHECK constraint
- Data synchronization
- Uniqueness assurance
- Query execution across tables
- Synchronize metadata
- Referential integrity management
- Identification of metadata tributary columns

### When we can use anti-pattern

An archive that separates past data from the latest data
The need to execute queries on historical data is greatly reduced

***Solution***

Perform partitioning and normalization

- Horizontal partitioning
- Sharding
- Vertical partitioning
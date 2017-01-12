---
layout: post
title:  "Introduction to Stored Procedures"
date:   2016-09-27 13:09:43 +0900
categories: Development
---

As engineers work, we often receive the following request.

- "Please show about this data"
- "I wanna see user log" etc

But, if the content of the request is complicated, normal SQL may not be able to deal with it.

Sometimes we face following situation.

- As a result it can be issued, but the SQL statement to be executed is divided into two or more.
- I can not get out in an ideal form well.

At that time, Stored procedure may be efficient.

## About stored procedure

Stored procedure is ...

> In a database management system (DBMS),
> a stored procedure is a set of Structured Query Language (SQL) statements with an assigned name that's stored in the database in compiled form so that it can be shared by a number of programs. 
> qt: [http://searchoracle.techtarget.com/definition/stored-procedure](http://searchoracle.techtarget.com/definition/stored-procedure)

The following article was helpful for how to use this.

[https://dev.mysql.com/doc/refman/5.6/en/create-procedure.html](https://dev.mysql.com/doc/refman/5.6/en/create-procedure.html)

And I created following stored procedure for practice.

- ① a month's data from the specified date using the while statement
- ② a table that counts the number of participants for the event using the cursor

This is sample.
I'm not sure it is practical or not.

In addition, this sample uses MySQL.

## ① a month's data from the specified date using the while statement

First, I created this table.


    CREATE TABLE [DB名].`event` (
      `id` INT NOT NULL,
      `start_date` DATETIME NOT NULL,
    PRIMARY KEY (`id`));


Next, I write stored procedure like the one below.

{% gist yukihirai0505/c8e100b41760740ca668 %}

I write comments in the code.
It creates data for one month from the specified date by dynamically creating SQL and turning through while executing it.

How to call this procedure is like this

    call {procedure name}("2016-02-21");

## ② a table that counts the number of participants for the event using the cursor

I would like to dynamically create a table that takes the ID of each event in the column based on the above data.
Here I used the cursor.

It may be difficult to use cursor at the first time.
But I think that it is easy to understand if you think separating each one.

This cursor saves the data contained in a table and loops the result of each record.
It plays a role like a for statement.

{% gist yukihirai0505/79b2a87d40976ec20853 %}

Like above program,
I could create a table with all eventId in the column
while saving id's data from the event table created in ① to the cursor and turning its id with a loop.

Initially I felt a lot of things to remember, so I am confused.
But after understanding how to use stored procedure, it is very useful :)
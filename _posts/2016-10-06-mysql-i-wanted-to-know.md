---
layout: post
title:  "Things I think I should have known about using MySQL at work"
date:   2016-10-06 13:09:43 +0900
categories: Development
---

Until now I have not had the opportunity to touch SQL as much as I can depend on ORM,
but since the opportunity to touch SQL has increased about 1,000 times in the last couple of weeks,
I also write about MySQL as a memorandum.
Originally I wrote SQL itself before I became an engineer,
but I will write some pretty basic things I did not know at that time,
so I think that anyone in the middle of SQL will get nothing. lol
Rather, I'm happy if you give me any advice :)

## Writing simple SQL

### To use alias to be simple the table name

This might be different for individuals,
but if you omit the table name as `user u`, it will be easier to maintain.

{% gist yukihirai0505/f2771898c7ad7bb6e9ff %}

By doing this,
it is easy to see it when you look back and tune afterwards and it is neat.

## CASE expression

It is used to change what to display depending on conditions.

{% gist yukihirai0505/695025bc2df53ff2e213 %}

### How to sort by multiple conditions using ORDER BY

    ORDER BY s.start_date desc, s.id desc

### GROUP BY also specifies multiple conditions like ORDER BY

    GROUP BY s.start_date desc, u.id

### Partial replacement

{% gist yukihirai0505/749deecc8fb6f9909817 %}

## Tuning edition

Queries to be optimized are found in slow query logs and query analyzers.

[http://stackoverflow.com/questions/22609257/how-to-enable-mysql-slow-query-log](http://stackoverflow.com/questions/22609257/how-to-enable-mysql-slow-query-log)

If you installed MySQL by Homebrew,
You can check find conf file by using a following command.

    ls $(brew --prefix mysql)/support-files/my-*

And then, you can set up `my.cnf` file

    sudo cp $(brew --prefix mysql)/support-files/my-default.cnf /etc/my.cnf

### EXPLAIN


EXPLAIN used for data tuning adds EXPLAIN before the query and shows the execution result in a table.
However, I think that you can not understand the contents much by looking at this table at first look.

So you should remember an important thing that is `type` column.

If type part is index or ALL, there seems to be room for tuning.
From there, ALL's are optional, but it is good to put an index on the target column.

{% gist yukihirai0505/2234462dc5858e2c27b9 %}

<h2>When you want to compile logs in xxxx minutes</h2>

{% gist yukihirai0505/a8c1e31ec1d453e868d4 %}

It is good to first `SET` Something to be used repeatedly.

    UNIX_TIMESTAMP

=> It converts the date to unixtime (UTC's number of elapsed seconds since 01:00:00 on January 1, 1970).

    FROM_UNIXTIME

=> A function that can return the time stored in unixtime in an appropriate format.

## How to sort with UNION

When you UNION a table of A and a table of B,
it may not be reflected even if you order by within the table of B.
If sorting in the table of B is further selected and defined in another table,
we can do UNION with sorting in effect.

{% gist yukihirai0505/f30095bd5d907e49ef2a %}

## Understanding Transactions

In a situation where a plurality of users operate the database at the same time,
a transaction is a data inconsistency even if an error occurs temporarily while executing a plurality of operations (selection, update, deletion) or the like on the database
It is a mechanism of a relational database system (RDBMS) that guarantees nothing.


### SELECT...FOR UPDATE

With a SELECT FOR UPDATE query in a transaction, an exclusive lock is applied to the row SELECTed until the end of the transaction.

## How to import data to Mysql from CSV file

First, you should use `--local_infile` option to import csv file.

    mysql --local_infile=1 -uroot -p

If you forget the option,
you will have a following error.

    ERROR 1148 (42000): The used command is not allowed with this MySQL version

And then,

{% gist yukihirai0505/76fff4aa6a4ada62be0c %}

However, depending on the file there is a CR line feed code.
In that case, importing with this will cause only one row to be imported.
So, if you convert the line feed code from CR to LF, it solves it.
Although it is good to convert with your text editor, let's use it because there is a useful command called nkf command.

If you are using Mac, you can install it by using a following command.

`brew install nkf`

You can convert line feed code from `\r` to `\n`.

`nkf -Lu [target file] > [after file name]`

Once conversion is completed, I think that all file's rows can be imported properly.

## How to insert the selected result

It is also useful.

[MySql insert the results of a select](http://stackoverflow.com/questions/4472929/mysql-insert-the-results-of-a-select)

## Summary

I still have nothing to know about MySQL and I write a badly informative SQL.
I will try to be able to write a little better SQL.
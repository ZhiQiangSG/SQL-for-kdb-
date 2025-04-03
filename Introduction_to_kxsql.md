SQL Interface - Introduction to KX SQL
=========

## Learning objectives
1. What is KX SQL and why is it useful?
2. How to run basic queries with KX SQL?
    1. SQL prompt `s)`
    2. SQL evaluate `.s.e`
3. SQL language supported
    1. Operators
    2. Logical Operators
    3. Select
    4. Aggregates
4. Table creation, modification and deletion
    1. `CREATE`
    2. `INSERT`
    3. `DROP`
5. Applying parameters
    1. Multiple parameters
    2. Single parameters
6. Pre-parse optimization
7. Interoperability

## Course Prerequisites
This course assumes the user has a basic knowledge of SQL. If you are new to SQL don't worry - you can check out [W3 Schools](https://www.w3schools.com/sql/) for some fantastic learning materials to get you up and running.

You do not need to have any prior knowledge of the q language.

If you are new to this KX Developer IDE you can checkout the DeveloperTips.md file for some quick tips on using this IDE and for a more detailed overview check out this free [Introduction to KX Developer Course](https://kx.com/academy/courses/introduction-developer/).

# Introduction
SQL, Structured Query Language, is a programming language designed to manage data stored in relational databases. SQL operates through simple, declarative statements.

The SQL language is widely used today across web frameworks and database applications. 

![](./images/kxsql/kxsql.png)

In order to harness the best performance from kdb+ the q language will always be the optimum way to query your data. But there is also now a way to run SQL queries using KX SQL. 

Given that most widespread and popular database query language is SQL, this opens up the power of KX to unlimited new users and industries.


# What is KX SQL and why is it useful?
KX SQL is a new SQL interface for KX. It is hugely powerful as it means SQL can be used to query kdb+ data without any knowledge of the q language.

It is not a replacement for q but is a complement to enable newcomers to KX to get up and running fast using a query language they are already familiar with.

Some of the main KX SQL operators we will be looking at in this course are:

| KX SQL operator |  |
|----------|--------------|
|`s)`    | Prompt that can be prefixed to a statement to invoke SQL  |
|`.s.e`     | Function that can be prefixed to a statement with double quotes to invoke SQL |
|`.s.sp`    | For parameter injection from q types        |
|`.s.sq`     | To pre-can the parsed version of an SQL query          |
|`.s.sx`    | Execute a pre-canned parsed version of an SQL query              |

[Go here for more documentation on using SQL with KX.](https://code.kx.com/insights/cloud-edition/sql.html#using-sql)

_Tip: Dot naming notation like `.s.e` is commonly seen in q and is a way to define [namespaces](https://code.kx.com/q/ref/#namespaces). In this instance any function prefaced with `.s.` will be a function related to SQL implementation._


# How to run basic queries with KX SQL?

### Dataset 

The taxi database details cab fares in New York City. The data was provided from the [NYC Taxi & Limousine Commission](https://www1.nyc.gov/site/tlc/about/tlc-trip-record-data.page). The database we have loaded contains trip data starting at January 1st, 2009 until March 31st, 2009. The taxi database is stored in a table called trips.

## Running KX SQL
There are 2 main ways to execute SQL queries with KX SQL:
* SQL prompt `s)`
* SQL evaluate `.s.e`


## SQL Prompt  `s)`

* Prefix your SQL query to execute SQL rather than q
* Most convenient to use - only need to add to beginning of line
* Can't save results with this method - can only execute and print out

```q
// SQL query using s)
s)SELECT * FROM trips WHERE date=2009.01.01
```
Note that when in a terminal or CLI tool, by default you will see the `q)` prompt :
```
q)
```
When you want to execute SQL, `s)` can be added to ensure the q interpretor does the required conversion:
```
q)s)
```

## SQL Evaluate `.s.e`
* Can be added to beginning of SQL query but you also need to wrap the query in double quotes
* A little more effort to add than s) - as you need to add double quotes to both the beginning and end of query
* The benefit is that you can save results to a variable with this method

```q
// SQL query using .s.e
.s.e"SELECT * FROM trips WHERE date=2009.01.01"
```
Given there are two methods, how do we know when to use `s)` or `.s.e`?

One main difference is with the first method `s)` we cannot save the results to a variable. So this method is only useful if we want to execute some code without saving its result.

The second method `.s.e` allows us to save the output.

### Assigning a value


Assignment in q is done using the [colon operator `:`](https://code.kx.com/q/ref/assign/#simple-assign)

In comparison let's try the same thing but using `.s.e` instead:

```q
// Can save results to a variable trips01
trips01:.s.e"SELECT * FROM trips WHERE date=2009.01.01"
```

```q
// Can save results to a variable vendorDist
vendorDist:.s.e"SELECT vendor,distance FROM trips WHERE date=2009.01.01"
// And then use that variable later on
.s.e"SELECT * FROM vendorDist"
```

NOTE: This will not work with the `s)` prompt:

```q
// Can't save results to a variable
// Throws an error
s)trips01:SELECT * FROM trips WHERE date=2009.01.01
```
```q
// Can't save results to a variable
// Throws an error
trips01:s)SELECT * FROM trips WHERE date=2009.01.01
```


Whether you decide to use `s)` or `.s.e` is user perference, but if you want to save the output you will need to use `.s.e`.

**Quiz Time!**

*Try Exercises 1-4 in kxsql.quiz -> kxsqlQuiz_noSolutions.md to test your understanding of how to run SQL queries.*

# SQL language supported

There has been significant effort to ensure `KX SQL` is compliant with [ANSI SQL](https://code.kx.com/insights/cloud-edition/sql.html#compliance-with-ansi-sql). This means we conform to ANSI standards for SQL specifications.

Let's take a look at some of the main supported SQL language and functions.

## [Operators](https://code.kx.com/insights/cloud-edition/sql.html#operators)

A full list of SQL operators can be found [here](https://www.w3schools.com/sql/sql_operators.asp).  The ones that `KX SQL` supports includes:
| Operator | Description                                                  |
|----------|--------------------------------------------------------------|
| +        | Add                                                          |
| -        | Subtract                                                     |
| *        | Multiply                                                     |
| /        | Divide                                                       |
| =        | Equals to                                                    |
| >        | Greater than                                                 |
| <        | Less than                                                    |
| >=       | Greater than or equal to                                     |
| <=       | Less than or equal to                                        |
| <>       | Not equal to                                                 |


To get the latest list of supported SQL operators please refer to [this](https://code.kx.com/insights/cloud-edition/sql.html#operators).

Let's run some simple examples of these operators.

### Add 
Adding fare and tip column and calling it `new_total`:
```q
// add +
.s.e"SELECT fare,tip,fare+tip as new_total 
        FROM trips"
```

### Subtract 
Subtract a month from the month column and call it `prev_month`:
```q
// subtract -
.s.e"SELECT month,month-1 as prev_month 
        FROM trips"
```

### Multiply

Assuming distance given is in miles let's multiple by 5280 to convert to feet calling in `distance_foot`:

```q
// multiply *
.s.e"SELECT distance,5280*distance as distance_feet
        FROM trips"
```

### Divide

Divide the fare column by 100 to convert from dollars to cents and call it `fare_cents`:

```q
// divide /
.s.e"SELECT fare, fare/100 as fare_cents
        FROM trips"
```
### Equals to
Return only records that had 2 passengers in the car:
```q
// equals to =
.s.e"SELECT * FROM trips where passengers=2"
```

### Greater than
Taking the same statement used for addition, let's apply a filter to only return records where the tip is greater than zero:
```q
// greater than >=
.s.e"SELECT fare,tip,fare+tip as total 
        FROM trips
        WHERE tip>0"
```

### Less than
Return journeys that cost less than 5 dollars:
```q
// less than <=
.s.e"SELECT * FROM trips WHERE fare<5"
```

### Greater than or equal to
Return journeys that were 12 miles or greater:
```q
// greater than or equal to >=
.s.e"SELECT * FROM trips WHERE distance>=12"
```

### Less than or equal to
Return journeys that cost less than or equal to 5 dollars:
```q
// less than or equal to <=
.s.e"SELECT * FROM trips WHERE fare<=5"
```

### Not equal to <> !=
Do not return any records where there was only 1 passenger in the car. 

```q
// not equal to <>
.s.e"SELECT * FROM trips WHERE passengers<>1"
```

```q
// not equal to !=
.s.e"SELECT * FROM trips WHERE passengers!=1"
```
 Note we have a few ways to do an not equal to comparison (see another using NOT in the next section).
 
The above examples are just a sample of the operators supported. For a full list of supported SQL operators please refer to [this](https://code.kx.com/insights/cloud-edition/sql.html#operators).

## Logical Operators
`KX SQL` also supports a number of [SQL logical operators](https://www.w3schools.com/sql/sql_operators.asp) including:
| Operator | Description                                                  |
|----------|--------------------------------------------------------------|
| AND      | True if all the conditions separated by AND are TRUE          |
| OR       | TRUE if any of the conditions separated by OR is TRUE        |
| NOT      | Displays a record if the condition(s) is NOT TRUE            |
| BETWEEN  | True if the operand is within the range of comparisons       |
| IN       | TRUE if the operand is equal to one of a list of expressions |
| LIKE     | TRUE if the operand matches a pattern                        |

Let's take a look at some of these in action.

### [AND](https://www.w3schools.com/sql/sql_and_or.asp)
Return only records for the DDS vendor where they had 3 passengers in the car:
```q
// AND
.s.e"SELECT * FROM trips
        WHERE passengers=3
            AND vendor='DDS'"
```

### [OR](https://www.w3schools.com/sql/sql_and_or.asp)

Return only records where the tip exceeded 20 dollars or the fare exceeded 100 dollars:
```q
// OR
.s.e"SELECT * FROM trips
        WHERE tip>20
            OR fare>100"
```


### [NOT](https://www.w3schools.com/sql/sql_and_or.asp)

This is a third option we have available in addition to `<>` and `!=` as previously mentioned. Let's take the sample example as before and only return records where there was only 1 passenger in the car.

```q
// NOT
.s.e"SELECT * FROM trips WHERE NOT passengers=1"
```


### [BETWEEN](https://www.w3schools.com/sql/sql_between.asp)

This operator selects values within a given range. The values can be numbers, text, dates or datetimes.

Let's first select all trips that cost between $10 and $12 dollars:
```q
// BETWEEN numbers
.s.e"SELECT * FROM trips
        WHERE fare BETWEEN 10 AND 12;"
```

Return all trips where `payment_type` falls alphabetically between 'CASH' and 'DISPUTE':
```q
// BETWEEN text
.s.e"SELECT * FROM trips
        WHERE payment_type BETWEEN 'CASH' AND 'DISPUTE';"
```
Let's return all records that fall between 2 dates:
```q
// BETWEEN dates
.s.e"SELECT * FROM trips
    WHERE date BETWEEN '2009-01-01' AND '2009-01-02;"
```
We actually only have one date, 1st January, in our dataset. Let's instead filter on datetimes.
```q
// BETWEEN datetimes
.s.e"SELECT * FROM trips
    WHERE pickup_time BETWEEN '2009-01-01 00:30:00' AND '2009-01-01 00:35:00';"
```

### [IN](https://www.w3schools.com/sql/sql_in.asp)

The IN operator allows you to specify multiple values in a WHERE clause. It is a shorthand for multiple OR clauses.

Return all records that are cash or credit.
```q
// IN
.s.e"SELECT * FROM trips
        WHERE payment_type IN ('CASH', 'CREDIT');"
```

### [LIKE](https://www.w3schools.com/sql/sql_like.asp)
The LIKE operator is used in a WHERE clause to search for a specified pattern in a column. In standard SQL syntax there are two wildcards only used:
* The percent sign `%` represents zero, one, or multiple characters
* The underscore sign `_` represents one, single character

Both of these are supported in `KX SQL`.

Let's return all records that have a `payment_type` beginning with 'C'.
```q
// LIKE with %
.s.e"SELECT * FROM trips
    WHERE payment_type LIKE 'C%';"
```
We can use `_` to search just leaving out one single character.
```q
// LIKE with _
.s.e"SELECT * FROM trips
    WHERE payment_type LIKE 'C_EDIT';"
```

    
## Select
We have been using the standard form of [select](https://www.w3schools.com/sql/sql_select.asp) up to this point. It allows us to select data from a database.

We can combine a number of other SQL operations with the select statement. The below are those supported, for a full list see [here](https://code.kx.com/insights/cloud-edition/sql.html#select).

| Operator | Description                                                  |
|----------|--------------------------------------------------------------|
| DISTINCT | Return only distinct (different) values                      |
| LIMIT    | Select a limited number of records                           |
| AS      | Give a table, or a column in a table, a temporary name |
| ORDER BY | Sort the result-set in ascending or descending order         |
| GROUP BY | Groups rows that have the same values into summary rows      |
| JOIN     | Combine rows from two or more tables                         |

Let's take a look at some of these in action.

### [DISTINCT](https://www.w3schools.com/sql/sql_distinct.asp)
The SELECT DISTINCT statement is used to return only distinct (different) values.

Return a unique list of vendor names in the trips table.
```q
// DISTINCT
.s.e"SELECT DISTINCT vendor FROM trips;"
```

### [LIMIT](https://www.w3schools.com/sql/sql_top.asp)
LIMIT can be used to select a limited number of records, which is useful on large tables to reduce the impact on performance.

Return only the first 5 records from table.
```q
// LIMIT
.s.e"SELECT * FROM TRIPS
        LIMIT 5;"
```
Note: that LIMIT with an offset is not yet implemented.

### [AS](https://www.w3schools.com/sql/sql_alias.asp)

AS can be used to give a table, or a column in a table, a temporary name.

Renaming the column vendor to be called company.
```q
// AS
.s.e"SELECT vendor as company FROM trips"
```
### [ORDER BY](https://www.w3schools.com/sql/sql_orderby.asp)

The ORDER BY keyword is used to sort the result-set in ascending or descending order. 

This keyword sorts the records in ascending order by default. To sort the records in descending order, use the DESC keyword.

Sort the records in ascending `payment_type` order.

```q
// ORDER BY
.s.e" SELECT * FROM trips
        ORDER BY payment_type;"
```

Sort the records in descending `payment_type` order.

```q
// ORDER BY with DESC
.s.e" SELECT * FROM trips
        ORDER BY payment_type DESC;"
```

### [GROUP BY](https://www.w3schools.com/sql/sql_groupby.asp)
The GROUP BY statement groups rows that have the same values for one or more columns or expressions into summary rows.

This statement is often used with aggregate functions. Check out the next section on supported aggregated functions to see what we can use GROUP BY with.

Return a count of number of trips per `payment_type`.
```q
/// GROUP BY
.s.e"SELECT payment_type,
            COUNT(payment_type) AS count_per_type
        FROM trips
        GROUP BY payment_type"
```
ORDER BY and GROUP BY expressions may be column names, ordinal numbers of the output columns, or arbitrary expressions.

### [JOIN](https://www.w3schools.com/sql/sql_join.asp)

A JOIN clause is used to combine rows from two or more tables, based on a related column between them.

Join types supported include: left, right, inner, and cross.

In order to showcase joins we will need a second table - let's create a new table now.

```q
/// create table called summary and cash_credit
summary:.s.e"SELECT payment_type,
            COUNT(payment_type) AS count_per_type
        FROM trips
        GROUP BY payment_type";
        
cash_credit:.s.e"SELECT * FROM summary WHERE payment_type IN ('CASH','CREDIT')";
```

#### [Left Join](https://www.w3schools.com/sql/sql_join_left.asp)
Returns all records from the left table, and the matched records from the right table.

Let's join `cash_credit` to `trips` as a left join, based on matching `payment type` values.

```q
// Left Join
.s.e"SELECT *
    FROM trips
    LEFT JOIN cash_credit 
        ON trips.payment_type = cash_credit.payment_type;"
```
Note that where there were no matches records were returned but joined fields are blank.

#### [Right Join](https://www.w3schools.com/sql/sql_join_right.asp)
The RIGHT JOIN keyword returns all records from the right table, and the matching records from the left table. 

Let's do the previous example again but as a right join, we will also need to swap the tables around.

```q
// Right Join
.s.e"SELECT *
    FROM cash_credit
    RIGHT JOIN trips 
        ON trips.payment_type = cash_credit.payment_type;"
```

#### [Inner Join](https://www.w3schools.com/sql/sql_join_inner.asp)
The INNER JOIN keyword selects records that have matching values in both tables.

Doing an inner join on the `cash_credit` table returns only records matched on the `payment_type` column.
```q
// Inner Join
.s.e"SELECT *
    FROM trips
    INNER JOIN cash_credit 
        ON trips.payment_type = cash_credit.payment_type;"
```
Note that the only records are returned are ones in both tables.

#### [Cross Join](https://www.w3resource.com/sql/joins/cross-join.php)
The CROSS JOIN produces a result set which is the number of rows in the first table multiplied by the number of rows in the second table, if no WHERE clause is used along with CROSS JOIN. This kind of result is called as Cartesian Product.

Let's create another table `vendors` to demonstrate.
```q
// create table vendors
vendors:.s.e"SELECT vendor,payment_type,
            SUM(fare)
        FROM trips
        GROUP BY payment_type,vendor";
```

Let's cross join that with our `cash_credit` table and see what happens.
```q
// Cross Join
cross_tab:.s.e"SELECT * 
            FROM cash_credit 
            CROSS JOIN vendors;"
```
Each row from the first table joins with each row of the second table such that: 

| table                     | number rows |
|---------------------------|-------------|
| table1                    | x           |
| table2                    | y           |
| table1  CROSS JOIN table2 | x*y         |

Which in our example gives:
| table                           | number rows |
|---------------------------------|-------------|
| cash_credit                     | 2           |
| vendors                         | 6           |
| cash_credit  CROSS JOIN vendors | 2*6=12      |
## Aggregates
The SQL aggregates that are supported in `KX SQL` are listed here:
| Aggregate |
|-----------|
| COUNT     |
| AVG       |
| COUNT(*)  |
| FIRST     |
| LAST      |
| MAX       |
| MIN       |
| SUM       |

Note: Please keep up to do with new aggregates that maybe be added [here](https://code.kx.com/insights/cloud-edition/sql.html#aggregates).

We have already seen at least one of these in action - COUNT. 

Let's add in some others.
```q
// Aggregates 
.s.e"SELECT AVG(fare), 
            SUM(tip), 
            MIN(tolls), 
            MAX(total),
            COUNT(passengers) 
        FROM trips;"
```
# Table Creation, Modification and Deletion
`KX SQL` supports the use of [Data Definition Language (DDL)](https://en.wikipedia.org/wiki/Data_definition_language) which can be used to handle the database descriptions and schemas. It can also be used to define and modify the structure of the data.

## CREATE
[CREATE](https://www.w3schools.com/sql/sql_create_db.asp) can be used to create a new table in kdb+.
Purely using SQL we are able to create tables. 

Check out the full list of [datatypes](https://code.kx.com/insights/cloud-edition/sql.html#datatype-conversions) supported.
```q
// creating an empty table of 2 columns 
s)CREATE TABLE tripsFare (vendor varchar,fare float)
s)SELECT * FROM tripsFare
```

Note: Above we are using varchar to define a column of q type symbol. We could also have used char(n>1) here. If we wanted to define a column of q type char - we would use char(1).


## INSERT
[INSERT](https://www.w3schools.com/sql/sql_insert.asp) can be used to add new records to a table.

Next, let's add some data to our table `tripsFare` that we just created in the previous step.

Adding one row:
```q
s)INSERT INTO tripsFare(vendor,fare) values ('CMT',100)
s)SELECT * FROM tripsFare
```
Adding two rows:

```q
s)INSERT INTO tripsFare(vendor,fare) values ('DDS',301),('CMT',589)
s)SELECT * FROM tripsFare
```
## DROP

[DROP](https://www.w3schools.com/sql/sql_drop_table.asp) can be used to delete a kdb+ table.

Let's delete our newly created `tripsFare` table and see what happens when we try to call it:
```q
// dropping table - no longer exists
// get error can't find
s)DROP table IF EXISTS tripsFare
s)SELECT * FROM tripsFare
```

Please note at the time of the creation of this course the SQL statements `UPDATE` and `DELETE` are [`nyi`](https://code.kx.com/q/basics/errors/). Check here for documentation on latest [SQL statements](https://code.kx.com/insights/cloud-edition/sql.html#other-sql-statements) available. 

For a 'how to' on updating and deleting rows and columns in your table using q you can check out the [KX Academy](https://kx.com/academy/courses/queries-qsql/).

**Quiz Time!**

*Try Exercises 7-9 in kxsql.quiz -> kxsqlQuiz_noSolutions.md to test your understanding of using `CREATE`, `INSERT` and `DROP`.*


# Applying Parameters
A parameter will allow you to provide a value to pass into a predefined query. Parameters allow you to come up with scenarios or options that are not available in your data and create these values to put into your code. 

They are super useful because after creation, end users can control the input to see the results of the parameters effect.

## Parameters for KX SQL
The `.s.sp` routine may be used for parameter injection from q types. In the sections above the parameters used in the WHERE clauses were static - next we will take a look at making those parameters dynamic.

We can pass in dynamic parameters to our SQL query using `.s.sp` and `$n` when listing the variables.

## Multiple parameter queries
Let's look at both the static and dynamic way to return only fares that exceeded $20 for DDS vendor.

_Static Parameters_
```q
// regular sql query with static parameters
.s.e"SELECT vendor,fare FROM trips WHERE vendor='DDS' AND fare>20"
```
We can see only fares for DDS that are greater than $20 are now returned.

_Dynamic Parameters_

Next, to make these parameters dynamic parameters we would:
* Use `.s.sp` in place of `.s.e`
* Wrap square brackets around the query
* Pass the parameters at the end in q syntax - note the symbol backtick for `DDS now used instead of SQL syntax 'DDS'
    * Check out the full list of [KX SQL datatypes](https://code.kx.com/insights/cloud-edition/sql.html#datatype-conversions) to see what a varchar is in `q`
    * Then you can refer to [KX Datatypes](https://code.kx.com/q/basics/datatypes/) to see a sample of how they are defined
    * A helpful introduction to KX Datatypes can also be found in this [course](https://kx.com/academy/courses/atoms-primitives2/) if you would like to do some further learning
* Refer to parameters as `$n` in the query
```q
// using .s.sp in place of .s.e to pass dynamic parameters $1 and $2
.s.sp["SELECT vendor,fare FROM trips WHERE vendor = $1 AND fare>$2"](`DDS;20)
```

We get the same thing as using the static parameters previously, except now we have the added benefit of dynamic input parameters.

## Single parameter queries
There are some important nuances to be aware of when using only a single parameter - primarily a single parameter needs to be converted into a single item list.

```q
// only having one parameter
// fails with 'rank error
.s.sp["SELECT vendor,fare FROM trips WHERE fare>$1"](50)
```
We need to add enlist when we are only passing one parameter. The reason for this is `.s.sp` is expecting a list as its input, the function [enlist](https://code.kx.com/q/ref/enlist/) is used here to make a single item list.
```q
// adding enlist to my parameter
.s.sp["SELECT vendor,fare FROM trips WHERE fare>$1"](enlist 50)
```
Success! 


**Quiz Time!**

*Try Exercises 5-6 in kxsql.quiz -> kxsqlQuiz_noSolutions.md to test your understanding of the use dynamic parameters with SQL queries.*


# Pre-parse Optimization

The default behaviour of `KX SQL` and `.s.e` is to parse the query from SQL to q and then execute the whole statement everytime it is run. 

This means that for highly repeated queries, say running an analytic or running from a dashboard, the query is parsed everytime its is executed. This parsing step adds significant, and unnessecary, overhead when it is really only required once.

For this scenario the functions `.s.sq` and `.s.sx` can be used to allow parsing of the query to be done only once and then just the execution is executed repeatedly.

Taking a simple example where we want to return all columns from our trips table for 1st Jan 2009. 

We are using [\t timer](https://code.kx.com/q/basics/syscmds/#t-timer) to show how long the query took in milliseconds - this time includes both parsing and execution.
```q
//Standard SQL Query - run once
\t .s.e["SELECT * FROM trips WHERE date =2009.01.01"]
```
Running this once takes 4-5 milliseconds so is relatively good performance wise. But when we add `\t:100` we are running it 100 times in a row.

```q
//Standard SQL Query - run 1000 times
\t:1000 .s.e["SELECT * FROM trips WHERE date =2009.01.01"]
```
This takes significantly longer around 4-5 seconds. Let's see if we can improve this using `.s.sp` and `.s.sx`.
## `.s.sq`
The function `.s.sq` can be used to pre-parse the SQL.

The function requires that you pass it an empty table schema and use parameters to call the query. This can be done using the `.s.e` function to return the table schema.
```q
//SQL set up - get table schema
tabSchema:.s.e"SELECT * FROM trips LIMIT 0"
```
Now the table schema is assigned to `tabSchema` we can call `s.sq` passing in the SQL statement we want to pre-parse and the table schema we just defined.

```q
//Create the pre-parsed SQL query 
//includes two dynamic parameters 
parsedQuery:.s.sq["SELECT * FROM $1 WHERE date =$2";(tabSchema;0Nd)];
```

Now we are ready to execute our query using `.s.sx`.
## `.s.sx`

```q
// Running the same query as before, but now with `.s.sx` instead of `.s.e` and passing in our two parameters
\t .s.sx[parsedQuery](`trips;2009.01.01)
```
We can see that this has gone from ~5 milliseconds down to 0 milliseconds.
```q
// Again now but running 1000 times
\t:1000 .s.sx[parsedQuery](`trips;2009.01.01)
```
The improvement is greater the more iterations we do - we can see now a reduction from ~5 seconds to ~300 milliseconds.

Pre-canned SQL queries are faster. The usage of the functions `.s.sq` and `.s.sx` consistently provides faster query speeds regardless of iterations.

**Quiz Time!**

*Try Exercise 10 in kxsql.quiz -> kxsqlQuiz_noSolutions.md to test your understanding of improving performance of highly called queries using `.s.sq` and `.s.sx`*



# Interoperability

The SQL interface opens up a lot of new interoperability options to users. 

One of the main ways is through a Postgres interface, as this means you can call KX data in any application that supports Postgres.

![](./images/kxsql/interopsql.png)

This includes applications like Tableau, PowerBI, Grafana and DbVisualizer.

# Conclusion

Congratulations on completing this Introduction to SQL course! 

## Follow Along Tutorial

Head to kxsql.project -> Tutorial_employees_database.md for our follow along tutorial of the topics covered in this course.

## Challenge & Certificate 

To be marked complete and receive your certificate there are two final steps, submission of the challenge, and completion of the post course questionnaire.

The challenge can be found at kxsql.project -> Challenge_school_admission.md and instructions on submission and a link to the survey can also be found there.

We hope you enjoyed the course and found it worthwhile!

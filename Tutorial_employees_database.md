Guided Tutorial - Employees Database
=========

In this tutorial you will follow the step-by-step guide to create an employees database using kxsql.

We will be walking through the following steps:

* Creating tables
* Inserting records
* Querying
    * Basic queries
    * Joining tables
* Adding parameters to a query
    * Multiple parameters
    * Single parameters

## Background 
You have just joined a new company called Dunder Mifflin and your first task is to setup a employee database for the firm. You are already familiar with SQL and so choose it to build your database. 

# Creating tables

Create a table called `employees` that contains the following columns and [datatypes](https://code.kx.com/insights/cloud-edition/sql/#datatype-conversions):

| column     | datatype |
|------------|----------|
| employee_no |  integer |
| birth_date  | date     |
| first_name  | varchar  |
| last_name   | varchar  |
| gender     | varchar  |
| hire_date   | date     |

```q
// Creating table employees
.s.e"CREATE TABLE employees(
    employee_no int,
    birth_date date,
    first_name varchar,
    last_name varchar,
    gender varchar,
    hire_date date
    );"
```
Create a table called `salaries` that contains the following columns and [datatypes](https://code.kx.com/insights/cloud-edition/sql/#datatype-conversions):

| column     | datatype |
|------------|----------|
| employee_no |  integer |
| salary  | integer     |
| from_date  | date  |
| to_date   | date  |


```q
// Creating table salaries
.s.e"CREATE TABLE salaries(
    employee_no int,
    salary int,
    from_date date
    );"
```

# Inserting Records
There are currently 7 employees - lets add their details.
```q
// Inserting employees data into table
.s.e"INSERT INTO employees
      ( employee_no, birth_date, first_name, last_name, gender, hire_date )
  VALUES
      (00001, '1965-03-15', 'Michael', 'Scott', 'M', '1990-01-20' ), 
      (00002, '1974-08-30', 'Jim', 'Halpert', 'M', '1998-03-05' ),
      (00003, '1976-06-23', 'Pam', 'Beesly', 'F', '2000-10-14' ),
      (00004, '1971-06-23', 'Dwight', 'Schrute', 'M', '1995-01-04' ),
      (00005, '1974-11-24', 'Angela', 'Martin', 'F', '1997-11-01' ),
      (00006, '1980-07-23', 'Kelly', 'Kapoor', 'F', '2001-05-01' ),
      (00007, '1964-01-23', 'Creed', 'Bratton', 'M', '1987-01-04' );
  "
```
Next lets add their salary information.
```q
// Inserting salaries data into table
.s.e"INSERT INTO salaries
      ( employee_no, salary, from_date)
  VALUES
      (00001, 65000, '2003-01-20' ), 
      (00002, 40000, '2000-03-05' ),
      (00003, 33000, '2000-10-14' ),
      (00004, 40000, '2001-01-04' ),
      (00005, 57600, '2002-11-01' ),
      (00006, 27000, '2002-05-01' ),
      (00007, 34000, '2000-01-04' );
  "
```


Please note at the time of creation of this course the SQL statements `UPDATE` and `DELETE` are [`nyi`](https://code.kx.com/q/basics/errors/). Check here for documentation on latest [SQL statements](https://code.kx.com/insights/cloud-edition/sql/#other-sql-statements) available with kxsql. 

For a how to on updating and deleting rows and columns in your table using q you can check out the [KX Academy](https://kx.com/academy/courses/queries-qsql/).

# Querying 
## Basic Queries
Select all the rows in the employees table.
```q
// simple select
.s.e"SELECT * FROM employees;"
```

Show the top 5 salaries:
```q
// using order by to descend records 
// and limit to only return top 5
.s.e"SELECT salary
    FROM salaries
    ORDER BY salary DESC
    LIMIT 5; "
```
Return only the employees who joined the company after 1995.
```q
// using where to filter rows
.s.e"SELECT *
    FROM employees
    WHERE hire_date>1995.01.01;"
```

Let's get a count of how many female to male employees there are.
```q
// using count and group by to pivot on gender
.s.e"SELECT gender,
            COUNT(*)
    FROM employees
    GROUP BY
        gender;"
```

Get the maximum, minimum and average salaries for all employees.

```q
// using SQL max, min and avg functions
.s.e"SELECT
    MAX(salary) as max_sal,
    MIN(salary) as min_sal,
    AVG(salary) as avg_sal
    FROM salaries;"
```

## Joining tables
Show the average salaries per gender.
```q
// joining the salaries table to employees 
// and using round to get avg salary value
// and group to pivot on gender 
.s.e"SELECT
    gender,
    ROUND(AVG(salary), 0) AS avg_salary
    FROM employees AS t
    LEFT JOIN salaries AS s
    ON t.employee_no = s.employee_no
    GROUP BY gender
    ORDER BY avg_salary DESC;"
```
We can see the male average salary is higher than the female average salary as well as higher the average salary for all employees.

You report this to senior management as there is clearly some work to be done at Dunder Mifflin to address equality of pay.

# Adding Parameters
## Multiple Parameters
Create a query that will filter on employee start date and gender passing these as parameters.

First let's create the query as normal with static parameters.
```q
// using where to filter rows
.s.e"SELECT *
    FROM employees
    WHERE hire_date>1995.01.01 AND
          gender='F';"
```

Now using `.s.sp` instead of `.s.e` we can make these filter constraints parameters.`
```q
// using .s.sp instead of .s.e and adding square brackets
// passing parameters in round brackets and referring to them using $n
.s.sp["SELECT *
    FROM employees
    WHERE hire_date>$1 AND
          gender=$2;"](1997.01.01,`F)
```
Note the datatypes passed now need to be a [kdb+ datatype](https://code.kx.com/q/basics/datatypes/) rather than SQL as they are outside of the SQL statement.
* *hire_date*: SQL date 1995-01-01 becomes kdb+ date 1997.01.01
* *gender*: SQL varchar 'F' becomes kdb+ symbol `F

Changing only the parameters return only male employees who started after 1997.
```q
// passing different parameters we get differing results 
.s.sp["SELECT *
    FROM employees
    WHERE hire_date>$1 AND
          gender=$2;"](1997.01.01,`M)
```
Only one! Looks like Dunder Mifflin has been hiring more females than males recently.

## Single Parameters
Taking the same query as before lets remove the filter on gender to see all employees hired after 1997.

The `.s.sp` function expects a list of parameters. So when we pass only a single parameter we must make that a one item list. In q this can be simply done using the keyword [enlist](https://code.kx.com/q/ref/enlist/).
```q
// this requires the use of enlist when passing only one parameter
.s.sp["SELECT *
    FROM employees
    WHERE hire_date>$1;"](enlist 1997.01.01)
```

Congratulations you have completed this tutorial!
![](https://media.giphy.com/media/IwAZ6dvvvaTtdI8SD5/giphy.gif)

Next head to kxsql.project -> Challenge_school_admission.md to test your knowledge gained in this course and earn your certificate!

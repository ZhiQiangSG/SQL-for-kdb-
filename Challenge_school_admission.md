Challenge - School Admissions
=========

In this project you will be asked to complete some questions related to using SQL & KX.

## Background

You have been hired as a temp for a School Admissions office to better manage students details and results.


### Exercise 1

Create a table using SQL called `students` that contains the following columns and datatypes:
| column     | datatype |
|------------|----------|
| student_no |  integer |
| first_name  | varchar  |
| last_name   | varchar  |
| year   | integer     |
| house   | varchar     |

Save your SQL code to variable `ex1` inside double quotes as shown below.
```q
// your ans here
ex1:"<your SQL code here>";
.s.e ex1
```

```q
// check your ans
testSectionSql[`exercise1]
```

### Exercise 2

Create a table using SQL called `results` that contains the following columns and datatypes:
| column     | datatype |
|------------|----------|
| student_no |  integer |
| subject  | varchar  |
| mark   | integer  |

Save your SQL code to variable `ex2` inside double quotes as shown below.
```q
// your ans here
ex2:"<your SQL code here>";
.s.e ex2
```

```q
// check your ans
testSectionSql[`exercise2]
```
### Exercise 3
Insert the following student details into the table `students`:
```
00001, 'Harry', 'Potter', '1', 'Gryffindor'
00002, 'Ron', 'Weasley', '1', 'Gryffindor'
00003, 'Draco', 'Malfoy', '1', 'Slytherin'
00004, 'Hermione', 'Granger', '1', 'Gryffindor'
00005, 'Luna', 'Lovegood', '1', 'Ravenclaw'
```

Save your SQL code to variable `ex3` inside double quotes as shown below.
```q
// your ans here
ex3:"<your SQL code here>";
.s.e ex3
```

```q
// check your ans
testSectionSql[`exercise3]
```

### Exercise 4
Insert the following results details into the table `results`:
```
00001, 'Potions', 46
00001, 'Defence Against the Dark Arts', 100
00002, 'Potions', 44
00002, 'Defence Against the Dark Arts', 88
00003, 'Potions', 100
00003, 'Defence Against the Dark Arts', 75
00004, 'Potions', 91
00004, 'Defence Against the Dark Arts', 81
00005, 'Potions', 97
00005, 'Defence Against the Dark Arts', 79
```

Save your SQL code to variable `ex4` inside double quotes as shown below.
```q
// your ans here
ex4:"<your SQL code here>";
.s.e ex4
```


```q
// check your ans
testSectionSql[`exercise4]
```

### Exercise 5
Return the average marks for each subject - renaming mark column to be avg_mark as part of the query. The table returned from query should have 2 columns as follows:

| column     | datatype |
|------------|----------|
| subject |  varchar |
| avg_mark  | float  | 

Save your SQL code to variable `ex5` inside double quotes as shown below.
```q
// your ans here
ex5:"<your SQL code here>";
.s.e ex5
```


```q
// check your ans
testSectionSql[`exercise5]
```

### Exercise 6 
Show the average marks per house using SQL Join, call the new column avg_mark. This new value should be rounded down (i.e. contain no decimal values).

Save your SQL code to variable `ex6` inside double quotes as shown below.
```q
// your ans here
ex6:"<your SQL code here>";
.s.e ex6
```

```q
// check your ans
testSectionSql[`exercise6]
```

### Exercise 7
Create a query that will filter on the `students` for Gryffindor house, returning all columns. 

Save your SQL code to variable `ex7` inside double quotes as shown below.
```q
// your ans here
ex7:"<your SQL code here>";
.s.e ex7
```

```q
// check your ans
testSectionSql[`exercise7]
```

### Exercise 8 
Adjust the query from exercise 7 such that the house filter becomes be a dynamic parameter.

Save your SQL code to variable `ex8` inside double quotes and your q parameter to variable `param` as shown below.

```q
// your ans here
ex8:"<your SQL code here>";
param:<your q param here>;
.s.sp[ex8]param
```

```q
// check your ans
testSectionSql[`exercise8]
```

Congratulations you've completed all sections!

![](https://media.giphy.com/media/PXvCWUnmqVdks/giphy.gif)

# Submission
To submit your project, please run the below which will test the variables and functions you've created:
```q
submitProjectSql[]
```

If all tests pass, you will receive an email with a code that you can enter in the [course completion section](https://learninghub.kx.com/courses/sql-for-kdb/quizzes/kx-sql-course-completion/) back on the Academy, and your certificate will be automatically generated and available to download. 

We hope you enjoyed the course and found it worthwhile!

Don't forget to go back to the [course homepage](https://learninghub.kx.com/courses/sql-for-kdb/) and leave feedback. 

## Happy Coding!

# SQL Interface Exercises - with Solutions

### Exercise 1
Run a SQL query using `s)` to retrieve only the columns `date`,`vendor` and `duration` from `trips` table for 1st Jan 2009.
```q
// your code here
s)SELECT date,vendor,duration FROM trips WHERE date=2009.01.01
```

### Exercise 2
Run a SQL query using `.s.e` to retrieve only the columns `date`,`vendor` and `duration` from `trips` table for 1st Jan 2009.
```q
// your code here
.s.e"SELECT date,vendor,duration FROM trips WHERE date=2009.01.01"
```

### Exercise 3
Using SQL save the results of the query from exercise 2 to a variable called `vendorDuration`
```q
// your code here
vendorDuration:.s.e"SELECT date,vendor,duration FROM trips WHERE date=2009.01.01"
```

### Exercise 4
Using SQL query the `trips` table for the columns `vendor`, `duration` , `fare` and `tip` for all journeys where the tip exceeded $10.
```q
// your code here
.s.e"SELECT vendor,duration,fare FROM trips WHERE date=2009.01.01 AND tip>10"
```

### Exercise 5
Adjust the query from excercise 4 to make the values for the where clause dynamic instead of static.
```q
// your code here
.s.sp["SELECT vendor,duration,fare FROM trips WHERE date=$1 AND tip>$2"](2009.01.01;10)
```

### Exercise 6
Adjust the query from excercise 5 to include only the condition for date.
```q
// your code here
.s.sp["SELECT vendor,duration,fare FROM trips WHERE date=$1"](enlist 2009.01.01)
```

### Exercise 7
Using SQL syntax create a brand new table called `typePayment` that is empty with the following schema:
| payment_type | fare  |
|--------------|-------|
| varchar      | float |
```q
// your code here
s)CREATE TABLE typePayment (payment_type varchar,fare float)
```

### Exercise 8
Using SQL syntax add the following records to the `typePayment` table created in exercise 7.
| payment_type | fare     |
|--------------|----------|
| CASH         | 86363.30 |
| CREDIT       | 15557.70 |
| Dispute      | 104.80   |
| No Charge    | 711      |

```q
// your code here
s)INSERT INTO typePayment(payment_type,fare) values ('CASH',86363.30),('CREDIT',15557.70),('Dispute',104.80),('No Charge',711)
```
### Exercise 9
Using SQL syntax delete the table `typePayment`:
```q
// your code here
s)DROP table IF EXISTS typePayment
```

### Exercise 10
The following SQL query pulls out columns related to GPS coordinates and is being used for a Dashboard that plots the trips against a map of NYC.
```q
\t .s.e"SELECT date,vendor,start_long,start_lat,end_long,end_lat,duration FROM trips WHERE date=2009.01.01"
```
The query performs ok as a one of call taking around 20 milliseconds. However users of the Dashboard are complaining about poor performance and slow latency.

The Dashboard performance can be modelled as running the query 100 times.
```q
\t:100 .s.e"SELECT date,vendor,start_long,start_lat,end_long,end_lat,duration FROM trips WHERE date=2009.01.01"
```
Using `.s.sx` and `.s.sq` improve the performance of this.
```q
// your code here
tabSchema:0#select date,vendor,start_long,start_lat,end_long,end_lat,duration from trips
parsedQuery:.s.sq["SELECT date,vendor,start_long,start_lat,end_long,end_lat,duration FROM $1 WHERE date =$2";(tabSchema;0Nd)];
\t:100 .s.sx[parsedQuery](`trips;2009.01.01)
```

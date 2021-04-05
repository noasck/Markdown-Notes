# Aggregate Functions
## Overview
Aggregate functions compute a single result from a set of input values. The built-in general-purpose aggregate functions are listed in [Official Postgres Documentation](https://www.postgresql.org/docs/13/functions-aggregate.html).

Grouping operations, which are closely related to aggregate functions, are listed in [Table 9.59 ](https://www.postgresql.org/docs/current/functions-aggregate.html#FUNCTIONS-GROUPING-TABLE)

It should be noted that except for count, these functions return a **null value** when no rows are selected. In particular, sum of no rows returns null, not zero as one might expect, and array_agg returns null rather than an empty array when there are no input rows.


[Table 9.56](https://www.postgresql.org/docs/current/functions-aggregate.html#FUNCTIONS-AGGREGATE-STATISTICS-TABLE) shows aggregate functions typically used in statistical analysis.

## Grouping Operations

``` sql
GROUPING ( group_by_expression(s) ) → integer
```
Returns a bit mask indicating which GROUP BY expressions are not included in the current grouping set. Bits are assigned with the rightmost argument corresponding to the least-significant bit; each bit is 0 if the corresponding expression is included in the grouping criteria of the grouping set generating the current result row, and 1 if it is not included.

#### Example
``` sql
=> SELECT * FROM items_sold;
 make  | model | sales
-------+-------+-------
 Foo   | GT    |  10
 Foo   | Tour  |  20
 Bar   | City  |  15
 Bar   | Sport |  5
(4 rows)

=> SELECT make, model, GROUPING(make,model), sum(sales) FROM items_sold GROUP BY ROLLUP(make,model);

 make  | model | grouping | sum
-------+-------+----------+-----
 Foo   | GT    |        0 | 10
 Foo   | Tour  |        0 | 20
 Bar   | City  |        0 | 15
 Bar   | Sport |        0 | 5
 Foo   |       |        1 | 30
 Bar   |       |        1 | 20
       |       |        3 | 50
(7 rows)
```
## Examples of aggregate functions

### AVG
Returns **average** of selected field or grouped set of fields.

``` sql
SELECT AVG(num_field) FROM sample_table;
-- for example we get 4.123456 as a result
```
Additionally, you can use ```ROUND``` method:
``` sql
SELECT ROUND(AVG(num_field), 3) FROM payment;
-- ang here we get 4.123 as a result
```
### MIN
``` sql
SELECT day, MIN(temp) FROM inspection GROUP BY day;
```
### MAX
``` sql
SELECT day, MAX(temp) FROM inspection GROUP BY day;
```
### SUM
``` sql
SELECT type, SUM(price) FROM payment GROUP BY type;
```
### COUNT
``` sql
SELECT customer_id, COUNT(*) FROM order GROUP BY customer_id;
```
## HAVING clause

We often user ```HAVING``` clause in conjunction with the ```GROUP BY``` clause to filter group rows that do not satisfy a specified condition.

The ```HAVING``` clause sets the condition for group rows created by the ```GROUP BY``` clause after the ```GROUP BY``` clause applies. In other hand, the ```WHERE``` clause sets the condition for individual rows **before** ```GROUP BY``` clause applies.
#### Using template
``` sql
SELECT column_1, agg_func(column_2)
FROM table
GROUP BY column_1
HAVING condition;
```

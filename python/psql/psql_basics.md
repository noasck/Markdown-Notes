# PosqtgreSQL

### Psql commands

- **\?** - help.
- **\i path-to-file.sql** - execute from file.
- **\c** - connect 2 db.
- **\l** - lists db's.
- **\d** - list of tables and relations.
- **\dt** - lists only tables.
- **\d table** - list specific table.

### Tables and db's
``` sql
CREATE DATABASE sample;
DROP DATABASE sample;

CREATE TABLE table_name(
  columns
);

CREATE TABLE person (
  id BIGSERIAL NOT NULL PRIMARY KEY,
  first_name VARCHAR(50),
  date_of_birth DATE NOT NULL
);

DROP TABLE person;
```
### Insert
``` sql
INSERT INTO table(column_name) VALUES ('value1');

-- Example:
INSERT INTO person(first_name, last_name, date_of_birth)
VALUES ('Anne', 'Smith', date '1988-01-09');
 ```
### Select

``` sql
-- select all
SELECT * FROM table;
SELECT first_name, last_name FROM person;
```
### Order by, Where Clause and AND
``` sql
  SELECT * FROM table ORDER BY column_name ASC;
  SELECT * FROM table ORDER BY column_name DESC;

  SELECT * FROM table WHERE column_name = 'value';

  SELECT * FROM table WHERE column_name1 = 'value1' AND
  (col_name2 = 'value2' OR col_name3 = 'value3');

  -- not equal operation:
  1 <> 2;
```
### Limits
Sets the max count of result rows.
``` sql
-- returns first 5 rows
Select * FROM table LIMIT 5;

-- returns from 6th to 10th rows
SELECT * FROM table OFFSET 5 LIMIT 5;

-- do the same: SQL standard
SELECT * FROM table OFFSET 5 FETCH FIRST 5 ROW ONLY;
```
### In, Between, Like, iLike
``` sql
-- one of list values
SELECT * FROM table WHERE field IN ('Value1', 'Value2', 'Value3');

-- between values
SELECT * FROM table WHERE date_of_birth BETWEEN DATE '2000-01-01' AND '2015-01-01';

-- prefix or suffix string match
SELECT * FROM table WHERE field LIKE '%suffix';
SELECT * FROM table WHERE field LIKE 'prefix%';
SELECT * FROM table WHERE field LIKE '%both%';
SELECT * FROM table WHERE field LIKE '_1char';

-- NOT case sensitive
SELECT * FROM table WHERE field ILIKE 'pREfix%';
```
 \'_' in LIKE expression matches single char

### When we are using the FROM Clause it is mandatory or necessary to use an alias(AS) in a query but when we are using any other clause like having, when, etc... not necessary to use an alias(AS). 
## 1 - Introduction to  SQL and installation of SQL server_client

* Create the table for amazon_orders -- 

```sql

--Different Types of Commands
-- DDL -> Data Definition Language
CREATE TABLE amazon_orders
(
order_id integer,
order_date date,
product_name varchar(50),
total_price decimal(6,2),
payment_method varchar(20)
);
-- Delete a Table
DROP TABLE amazon_orders;

--DML -> Data Manipulation Language(INSERT, UPDATE, DELETE, CALL, EXPLAIN CALL, LOCK)
INSERT INTO amazon_orders VALUES(1, '2023-23-09', 'Baby Milk', 30.5, 'UPI');
INSERT INTO amazon_orders VALUES(2, '2023-24-10', 'Baby Powder', 130, 'Credit Card');
INSERT INTO amazon_orders VALUES(3, '2023-25-10', 'Baby Cream', 30.5, 'UPI');
INSERT INTO amazon_orders VALUES(4, '2023-26-10', 'Baby Soap', 130, 'Credit Card');
DELETE FROM amazon_orders;
--SQL--> Structured Query Language
-- DQL -> Data Querying Language(SELECT)
SELECT * FROM amazon_orders;

--Limiting Columns or Selecting Specific Columns
SELECT product_names, order_id, order_date FROM amazon_orders;

--Limiting Rows
SELECT Top 1 * FROM amazon_orders;

--Data Sorting
SELECT * FROM amazo_orders
ORDER BY order_date DESC
ORDER BY order_date DESC, product_name DESC;

-- Data Type--
-- integer -> 1,2,3
-- Date -> 20-01-2022
-- varchar(100) -> strings 'baby milk'
```
## 2 - ALTER commands, Constraints, Delete and Update
* Create the table for amazon_orders -- 

```sql
CREATE TABLE amazon_orders
(
order_id integer,
order_date date,
product_name varchar(50),
total_price decimal(6,2),
payment_method varchar(20)
);
SELECT * FROM amazon_orders;
-- DDL -> Data Definition Language(ALTER, CREATE, DROP, TRUNCATE)
-- Change the Data Type of a Column
ALTER TABLE amazon_orders ALTER COLUMN order_date DATETIME;
INSERT INTO amazon_orders VALUES(5, '2023-26-10 12:05:12', 'Shoes', 135.5, 'UPI');
INSERT INTO amazon_orders VALUES(6, '2023-26-10 12:05:12', null, 135.5, 'UPI', 'Drishti');
-- Add a Column in an Existing Table
ALTER TABLE amazon_orders ADD username varchar(20);
ALTER TABLE amazon_orders ADD category varchar(20);
-- Delete a Column from an Existing Table
ALTER TABLE amazon_orders DROP COLUMN username;
```
* Constraints(NOT NULL, CHECK, UNIQUE, DEFAULT, PRIMARY KEY, FOREIGN KEY)
```sql
CREATE TABLE amazon_orders
(
order_id integer NOT NULL UNIQUE,
order_date date,
product_name varchar(50),
total_price decimal(6,2),
payment_method varchar(20) CHECK (payment_method IN('UPI','CREDIT CARD'))
);
INSERT INTO amazon_orders VALUES(8, '2023-26-10 12:05:12', 'Shoes', 135.5, NULL);
SELECT * FROM amazon_orders;
-- Primary Keys have UNIQUE Value and NOT NULL Constraints. We can have only one Primary Key in a Table but we can have Multiple Unique constraints in a table. 
```
* Delete (We can use WHERE Clause but in Drop, we can't use WHERE Clause)
```sql
CREATE TABLE amazon_orders
(
order_id integer NOT NULL UNIQUE,
order_date date,
product_name varchar(50),
total_price decimal(6,2),
payment_method varchar(20) CHECK (payment_method IN('UPI','CREDIT CARD'))
);
DELETE FROM amazon_orders;
```
* Update
```sql
CREATE TABLE amazon_orders
(
order_id integer NOT NULL UNIQUE,
order_date date,
product_name varchar(50),
total_price decimal(6,2),
payment_method varchar(20) CHECK (payment_method IN('UPI','CREDIT CARD'))
);
UPDATE amazon_orders SET discount = 10;
UPDATE amazon_orders SET discount = 10 WHERE order_id = 2;
SELECT * FROM amazon_orders;
```
## 3- SELECT AND WHERE STATEMENT
```sql
SELECT Top 5 * order_id,order_date FROM orders ORDER BY order_date;
-- DISTINCT KEYWORD (To get the Distinct Value of a Keyword)
SELECT DISTINCT ship_mode FROM orders;
----------------Filters----------------
SELECT * FROM orders WHERE ship_mode = 'First Class';
SELECT * FROM orders WHERE order_date = '2020-12-08';
SELECT * FROM orders WHERE quantity = 5;
SELECT * FROM orders WHERE order_date BETWEEN '2020-12-08' AND '2020-12-12' ORDER BY order_date DESC;
SELECT * FROM orders WHERE ship_mode IN ('First Class', 'Same Day');
SELECT * FROM orders WHERE ship_mode NOT IN ('First Class', 'Same Day');
SELECT * FROM orders WHERE ship_mode = 'First Class' OR ship_mode = 'Same Day'; -- or filter always increase rows
SELECT * FROM orders WHERE quantity > 5 AND order_date < '2020-11-08'; -- and filter always decrease the rows
-----------Pattern Matching Like Operator---------------
SELECT order_id, order_date, customer_name FROM orders WHERE customer_name LIKE 'C%';
SELECT order_id, order_date, customer_name FROM orders WHERE customer_name LIKE '%D';
SELECT order_id, order_date, customer_name FROM orders WHERE customer_name LIKE '%C%';
SELECT order_id, order_date, customer_name FROM orders WHERE customer_name LIKE 'C%';
---%--- > 0 or more any characters
---_--- > one character
SELECT order_id, order_date, customer_name FROM orders WHERE customer_name LIKE 'C[albo]%';
SELECT order_id, order_date, customer_name FROM orders WHERE customer_name LIKE 'C[^albo]%';
SELECT order_id, order_date, customer_name FROM orders WHERE customer_name LIKE 'C[a-f]%';
```
## 4- SQL Filters Continue and Data Aggregation
* Write an SQL to get all the orders where the customer's name has 'a' as the second character and 'd' as the fourth character(58 rows)  -- 
```sql
SELECT * FROM orders WHERE customer's name LIKE '_a%d%';
```
* Write a query to find the Top 5 orders with the highest sales in the furniture category.
```sql
SELECT TOP 5 * FROM orders WHERE category='Furniture' ORDER BY sales DESC;
```
* Filtering Null Values
```sql
SELECT * FRPM orders WHERE city IS NULL ;
SELECT * FRPM orders WHERE city IS NOT NULL ;
```
* Aggregation
```sql
SELECT COUNT(*) AS cnt, SUM(sales) AS total_sales, MAX(sales) AS max_sales, MIN(profit) AS min_profit, AVG(profit) AS avg_profit FROM orders;
-------------Group By----------------------
SELECT region,COUNT(*) AS cnt, SUM(sales) AS total_sales, MAX(sales) AS max_sales, MIN(profit) AS min_profit, AVG(profit) AS avg_profit FROM orders GROUP BY region;
SELECT TOP 5 sub_category, sum(sales) AS total_sales FROM orders WHERE profit > 50 GROUP BY sub_category HAVING sub_category > 100000 ORDER BY total_sales DESC;
------------------Count Function----------------------------
SELECT COUNT(DISTINCT region), COUNT(*), COUNT(city) SUM(sales) FROM orders;
--------Count Function does not count NULL values ANY Aggregate function will ignore the null value.----------
```
* Database Joins
```sql
CREATE TABLE returns(
order_id VARCHAR(20),
return_reason VARCHAR(20)
);
SELECT * INTO namastesql.dbo RETURNS FROM returns;
SELECT o.order_id,r.return_reason FROM orders o INNER JOIN returns r ON o.order_id = r.order_id;
SELECT o.order_id,r.return_reason FROM orders o LEFT JOIN returns r ON o.order_id = r.order_id;
SELECT * FROM employee, dept ORDER BY employee.emp_id;
SELECT e.emp_id, e.epm_name, e.dept_id, d.dep_name FROM employee e INNER JOIN dept d ON e.dept_id=d.dept_id;
SELECT e.emp_id, e.epm_name, e.dept_id, d.dep_name FROM employee e RIGHT JOIN dept d ON e.dept_id=d.dept_id;
SELECT e.emp_id, e.epm_name, e.dept_id, d.dep_name FROM employee e RIGHT JOIN dept d ON e.dept_id=d.dept_id;
SELECT e.emp_id, e.epm_name, e.dept_id, d.dep_name FROM employee e FULL OUTER JOIN dept d ON e.dept_id=d.dept_id;
```
* Date Function
```sql
SELECT order_id, order_date, DATEPART(year, order_date) AS year_of_order_date FROM orders;
SELECT order_id, order_date, DATEPART(year, order_date) AS year_of_order_date FROM orders WHERE DATEPART(year, order_date)=2020;
SELECT order_id, order_date, DATEPART(year, order_date) AS year_of_order_date, DATENAME(month, order_date) AS month_of_order_date;
SELECT order_id,order_date, DATEADD(DAY, 5, order_date) AS oredr_date_5 FROM orders;
SELECT order_id,order_date, DATEDIFF(DAY, oredr_date, ship_date) AS date_diff_days FROM orders;
```
* String Function
```sql
SELECT order_id, customer_name, LEN(customer_name) AS len_name FROM orders;
---------- Len Function will count space, and special characters as well---------
SELECT order_id, customer_name, LEFT(customer_name, 4) AS name_4 FROM orders;
SELECT order_id, customer_name, RIGHT(customer_name, 5) AS name_5 FROM orders;
SELECT order_id, customer_name, SUBSTRING(customer_name, 4, 5) AS substr45 FROM orders;
SELECT order_id, customer_name, CHARINDEX(' ', customer_name) AS space_position FROM orders;
SELECT order_id, customer_name, CHARINDEX('n', customer_name, 5) AS n_position FROM orders;
SELECT order_id, customer_name, CONCAT(order_id,'-', customer_name) AS concat_string FROM orders;
SELECT order_id, customer_name, REPLACE(order_id, 'CA', 'PB') AS replace_string FROM orders;
SELECT order_id, customer_name, TRANSLATE(order_id, 'AG', 'TP') AS TRANSLATE_string FROM orders;
SELECT order_id, customer_name, TRIM(' Ankit Bansal ') AS trim_string FROM orders;
```
* NULL Handling Function
```sql
SELECT order_id, city, ISNULL(city, 'unknown') AS new_city FROM orders WHERE city IS NULL;
SELECT order_id, city, state, region, COALESCE(city, state, region, 'unknown') AS new_city FROM orders WHERE city IS NULL;
SELECT TOP5 order_id, sales, CAST(sales AS INT), ROUND(sales, 1) AS sales_int FROM orders;
```
## 5- Set Function (UNION ALL won't remove duplicates but other set operations will remove duplicates)
```sql
CREATE TABLE(
order_id INT,
region VARCHAR(10),
sales INT
);
INSERT INTO orders_west VALUES(1, 'west', 100),(2,'west',200);
INSERT INTO orders_east VALUES(3, 'east', 100),(4,'east',300);
SELECT * FROM orders_west UNION ALL SELECT * FROM orders_east;
-----------UNION Will remove the duplicates----------------
SELECT * FROM orders_west UNION SELECT * FROM orders_east;
---------------Intersect Operator-------------
SELECT * FROM orders_west INTERSECT SELECT * FROM orders_east;
--------------MINUS Operation will----------------
SELECT * FROM orders_west MINUS SELECT * FROM orders_east;

```
* Case Statement
```sql
SELECT order_id, profit, CASE WHEN profit<100 THEN 'Low Profit' WHEN profit<200 THEN 'Medium Profit' WHEN profit<300 THEN 'High Profit' ELSE 'Very High Profit' END AS profit_category FROM orders;
```
## 6- SQL Interview Questions
* Write a query to produce the below output from icc_world_cup table. (team_name, no_of_matches_played, no_of_wins, no_of_losses.)
```sql
CREATE TABLE(
team_1 VARCHAR(20),
team_2 VARCHAR(20),
Winner VARCHAR(20)
);
INSERT INTO icc_world_cup VALUES('India', 'SL', 'India');
INSERT INTO icc_world_cup VALUES('SL', 'Aus', 'Aus');
INSERT INTO icc_world_cup VALUES('SA', 'Eng', 'Eng');
INSERT INTO icc_world_cup VALUES('Eng', 'NZ', 'NZ');
INSERT INTO icc_world_cup VALUES('Aus', 'India', 'India');
SELECT * FROM icc_world_cup;
SELECT team_name, COUNT(1) AS matches_played,  SUM(win_flag) matches_won, COUNT(1)-SUM(win_flag) AS lost_matches FROM (SELECT team_1 AS team_name, winner, CASE WHEN team_1 = winner THEN 1 ELSE 0 END AS win_flag FROM icc_world_cup UNION ALL SELECT team_2 AS team_name, CASE WHEN team_2 = winner THEN 1 ELSE 0 END AS win_flag FROM icc_world_cup) A GROUP BY team_name;
```
* View Table (It is a virtual table)(TimeTable will hold the data but ViewTable will not holding data)
```sql
CREATE VIEW orders_vw AS SELECT * FROM orders;
SELECT * FROM orders_vw;
```
* Referential Integrity Constraint (It will get created when there is the primary key in the table.)
```sql
SELECT * FROM emp;
SELECT * FROM dept;
CREATE TABLE emp(
emp_id INTEGER,
emp_name VARCHAR(20),
dept_id INT REFERENCES dept(dept_id)
);
INSERTINTO emp VALUES(1,'Ankit',100);
SELECT * FROM emp;
SELECT * FROM dept;
INSERTINTO emp VALUES(1,'Ankit',500);
```
* Identity (It will give incremental values so in the below code id will be auto-incremented)
```sql
CREATE TABLE dept1(
id INT IDENTITY(1,1),
dept_id INT,
dep_name VARCHAR(10)
);
INSERT INTO dept1(dep_id, dep_name) VALUES(100,'HR');
INSERT INTO dept1(dep_id, dep_name) VALUES(200,'Analytics');
SELECT * FROM dept1;
```
## 7- SUBQUERY And CTEs
* SUBQUERY
```sql
SELECT AVG(order_sales)AS avg_orders_value FROM (SELECT order_id, SUM(sales) AS order_sales FROM orders GROUP BY order_id) AS orders_aggregated;
SELECT * FROM employee WHERE dept_id NOT IN(SELECT dept_id FROM dept);
SELECT A.*, B.* FROM (SELECT order_id, SUM(sales) AS order_sales FROM orders GROUP BY order_id) A INNER JOIN (SELECT AVG(order_sales)AS avg_orders_value FROM (SELECT order_id, SUM(sales) AS order_sales FROM orders GROUP BY order_id) AS orders_aggregated) B ON 1=1;
```
* CTE(Common Table Expression)(Similar to Subquery, Readability is Good in CTEs, It makes query simiplar to look, We can use it multiple times)
```sql

```


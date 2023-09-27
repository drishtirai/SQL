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
----------------Filters-----------------
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

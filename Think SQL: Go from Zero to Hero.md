## 1 - Introduction to  SQL and installation of SQL server_client

* Create the table for amazon_orders -- 

```sql

--Different Types of Commands
-- DDL -> Data Defination Language
CREATE TABLE amazon_orders;
(
order_id integer,
order_date date,
product_name varchar(50),
total_price decimal(6,2),
payment_method varchar(20)
);
-- Delete a Table
DROP TABLE amazon_orders;

--DML -> Data Manipulation Language
INSERT INTO amazon_orders VALUES(1, '2023-23-09', 'Baby Milk', 30.5, 'UPI');
INSERT INTO amazon_orders VALUES(2, '2023-24-10', 'Baby Powder', 130, 'Credit Card');
INSERT INTO amazon_orders VALUES(3, '2023-25-10', 'Baby Cream', 30.5, 'UPI');
INSERT INTO amazon_orders VALUES(4, '2023-26-10', 'Baby Soap', 130, 'Credit Card');
DELETE FROM amazon_orders;
--SQL--> Structured Query Language
-- DQL -> Data Querying Language
SELECT * FROM amazon_orders;

--Limiting Columns or Selecting Specific Columns
SELECT product_names, order_id, order_date FROM amazon_orders;

--Limiting Rows
SELECT Top 1 * FROM amazon_orders;

--Data Sorting
SELECT * FROM amazo_orders
ORDER BY order_date DESC;
ORDER BY order_date DESC, product_name DESC;

-- Data Type--
-- integer -> 1,2,3
-- Date -> 20-01-2022
-- varchar(100) -> strings 'baby milk'
```

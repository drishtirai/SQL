# LEETCODE 50 MOST ASKED

## 1757. Recyclable and Low Fat Products

    SELECT product_id FROM Products WHERE low_fats='Y' AND recyclable='Y';

## 584. Find Customer Referee

    SELECT name FROM Customer WHERE referee_id IS NULL OR referee_id !=2;

## 595. Big Countries

    SELECT name, population, area FROM World WHERE area >= 3000000 OR population >= 25000000;

## 1148. Article Views 

    SELECT DISTINCT author_id AS id  FROM Views WHERE author_id = viewer_id ORDER BY id;

## 1683. Invalid Tweets

    SELECT tweet_id FROM Tweets WHERE LENGTH(content) > 15;

## 1378. Replace Employee ID With The Unique Identifier

    SELECT EmployeeUNI.unique_id, Employees.name
    FROM Employees
    LEFT JOIN EmployeeUNI ON Employees.id = EmployeeUNI.id;

## 1068. Product Sales Analysis 

    SELECT product_name, year, price FROM Sales s JOIN Product p ON s.product_id = p.product_id; 

## 1581. Customer Who Visited but Did Not Make Any Transactions

    
    

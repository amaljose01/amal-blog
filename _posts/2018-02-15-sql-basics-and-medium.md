---
layout: post
title: "SQL Basics and Medium Level: Complete Guide"
date: 2018-02-22 10:00:00 +0000
categories: [sql, database, tutorial, backend]
author: Amal
image: /assets/images/posts/2018-02-15-sql-basics-and-medium.svg
---



# SQL Basics and Medium Level: Complete Guide

## Introduction

SQL (Structured Query Language) is the standard language for managing relational databases. This guide covers everything from basic queries to intermediate concepts.

## Part 1: SQL Basics

### What is SQL?

SQL is used to:
- Create and manage databases
- Insert, update, and delete data
- Query data with SELECT statements
- Create relationships between tables

### Database and Table Creation

```sql
-- Create database
CREATE DATABASE company;
USE company;

-- Create table
CREATE TABLE employees (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE,
    salary DECIMAL(10, 2),
    department VARCHAR(50),
    hire_date DATE
);

-- Describe table structure
DESCRIBE employees;
```

### Basic CRUD Operations

```sql
-- CREATE (INSERT)
INSERT INTO employees (name, email, salary, department, hire_date)
VALUES ('John Doe', 'john@example.com', 50000, 'Engineering', '2018-01-10');

-- Multiple inserts
INSERT INTO employees (name, email, salary, department, hire_date) VALUES
('Alice Smith', 'alice@example.com', 55000, 'Engineering', '2018-01-15'),
('Bob Johnson', 'bob@example.com', 48000, 'Sales', '2018-02-01');

-- READ (SELECT)
SELECT * FROM employees;
SELECT name, email FROM employees;
SELECT * FROM employees WHERE department = 'Engineering';
SELECT * FROM employees ORDER BY salary DESC;
SELECT * FROM employees LIMIT 5;

-- UPDATE
UPDATE employees SET salary = 52000 WHERE name = 'John Doe';
UPDATE employees SET department = 'Management' WHERE salary > 50000;

-- DELETE
DELETE FROM employees WHERE id = 1;
DELETE FROM employees WHERE department = 'Sales';
```

### WHERE Clause and Operators

```sql
-- Comparison operators
SELECT * FROM employees WHERE salary > 50000;
SELECT * FROM employees WHERE salary >= 50000;
SELECT * FROM employees WHERE salary < 50000;
SELECT * FROM employees WHERE salary = 50000;
SELECT * FROM employees WHERE salary != 50000;

-- Logical operators
SELECT * FROM employees WHERE department = 'Engineering' AND salary > 50000;
SELECT * FROM employees WHERE department = 'Engineering' OR department = 'Sales';
SELECT * FROM employees WHERE NOT department = 'Engineering';

-- BETWEEN
SELECT * FROM employees WHERE salary BETWEEN 45000 AND 55000;

-- IN
SELECT * FROM employees WHERE department IN ('Engineering', 'Sales', 'HR');

-- LIKE (pattern matching)
SELECT * FROM employees WHERE name LIKE 'J%';
SELECT * FROM employees WHERE email LIKE '%@example.com';
```

### Sorting and Limiting

```sql
-- ORDER BY
SELECT * FROM employees ORDER BY salary;              -- Ascending
SELECT * FROM employees ORDER BY salary DESC;         -- Descending
SELECT * FROM employees ORDER BY department, salary;  -- Multiple columns

-- LIMIT and OFFSET
SELECT * FROM employees LIMIT 5;                      -- First 5 rows
SELECT * FROM employees LIMIT 5 OFFSET 10;            -- Skip 10, get 5
```

## Part 2: Intermediate SQL

### Aggregate Functions

```sql
-- COUNT
SELECT COUNT(*) FROM employees;
SELECT COUNT(email) FROM employees;  -- Counts non-NULL
SELECT COUNT(DISTINCT department) FROM employees;

-- SUM, AVG, MIN, MAX
SELECT SUM(salary) FROM employees;
SELECT AVG(salary) FROM employees;
SELECT MIN(salary) FROM employees;
SELECT MAX(salary) FROM employees;

-- GROUP BY
SELECT department, COUNT(*) as emp_count FROM employees GROUP BY department;
SELECT department, AVG(salary) as avg_salary FROM employees GROUP BY department;

-- HAVING (filter groups)
SELECT department, AVG(salary) as avg_salary 
FROM employees 
GROUP BY department 
HAVING AVG(salary) > 50000;
```

### JOINs

```sql
-- Create related table
CREATE TABLE departments (
    dept_id INT PRIMARY KEY,
    dept_name VARCHAR(100),
    location VARCHAR(100)
);

-- INNER JOIN
SELECT e.name, e.salary, d.dept_name
FROM employees e
INNER JOIN departments d ON e.dept_id = d.dept_id;

-- LEFT JOIN
SELECT e.name, d.dept_name
FROM employees e
LEFT JOIN departments d ON e.dept_id = d.dept_id;

-- RIGHT JOIN
SELECT e.name, d.dept_name
FROM employees e
RIGHT JOIN departments d ON e.dept_id = d.dept_id;

-- FULL OUTER JOIN
SELECT e.name, d.dept_name
FROM employees e
FULL OUTER JOIN departments d ON e.dept_id = d.dept_id;
```

### Subqueries

```sql
-- Subquery in WHERE
SELECT * FROM employees 
WHERE salary > (SELECT AVG(salary) FROM employees);

-- Subquery in FROM
SELECT * FROM (
    SELECT name, salary, department 
    FROM employees 
    WHERE salary > 50000
) high_earners;

-- Subquery with IN
SELECT * FROM employees 
WHERE department IN (
    SELECT dept_name FROM departments WHERE location = 'New York'
);
```

### UNION and Set Operations

```sql
-- UNION (removes duplicates)
SELECT name FROM employees WHERE department = 'Engineering'
UNION
SELECT name FROM employees WHERE salary > 60000;

-- UNION ALL (keeps duplicates)
SELECT name FROM employees
UNION ALL
SELECT name FROM employees WHERE salary > 50000;

-- EXCEPT
SELECT name FROM employees
EXCEPT
SELECT name FROM employees WHERE department = 'Sales';
```

### String Functions

```sql
SELECT UPPER(name) FROM employees;          -- Uppercase
SELECT LOWER(email) FROM employees;         -- Lowercase
SELECT LENGTH(name) FROM employees;         -- String length
SELECT SUBSTRING(name, 1, 3) FROM employees;  -- Extract substring
SELECT CONCAT(name, ' - ', email) FROM employees;  -- Concatenate
SELECT TRIM(name) FROM employees;           -- Remove whitespace
SELECT REPLACE(email, '@example.com', '@newdomain.com') FROM employees;
```

### Date Functions

```sql
SELECT CURDATE();                           -- Current date
SELECT CURTIME();                           -- Current time
SELECT NOW();                               -- Current timestamp

SELECT DATEDIFF(NOW(), hire_date) as days_employed FROM employees;
SELECT DATE_ADD(hire_date, INTERVAL 1 YEAR) FROM employees;
SELECT YEAR(hire_date) as hire_year FROM employees;
SELECT MONTH(hire_date) as hire_month FROM employees;
SELECT DAY(hire_date) as hire_day FROM employees;
```

### Views

```sql
-- Create a view
CREATE VIEW engineering_staff AS
SELECT name, email, salary 
FROM employees 
WHERE department = 'Engineering';

-- Query the view
SELECT * FROM engineering_staff;

-- Update view
ALTER VIEW engineering_staff AS
SELECT name, email, salary, hire_date
FROM employees 
WHERE department = 'Engineering';

-- Drop view
DROP VIEW engineering_staff;
```

### Indexes

```sql
-- Create index
CREATE INDEX idx_email ON employees(email);
CREATE INDEX idx_dept_salary ON employees(department, salary);

-- Drop index
DROP INDEX idx_email ON employees;

-- View indexes
SHOW INDEX FROM employees;
```

### Transactions

```sql
START TRANSACTION;

UPDATE employees SET salary = salary + 5000 WHERE department = 'Engineering';
UPDATE departments SET budget = budget - 100000 WHERE dept_name = 'Engineering';

-- Commit or Rollback
COMMIT;      -- Save changes
-- ROLLBACK;  -- Undo changes
```

## Summary

You've learned SQL from basics to intermediate level:
- **Basics**: CRUD operations, WHERE clauses, sorting
- **Intermediate**: JOINs, subqueries, aggregations, views, transactions

Practice these concepts with real databases to become proficient!

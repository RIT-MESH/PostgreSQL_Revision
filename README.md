# PostgreSQL Complete Guide 📘

## Overview 🚀
This repository contains a **comprehensive guide to PostgreSQL**, covering topics from **basic SQL queries** to **advanced concepts** like indexing, stored procedures, triggers, performance tuning, and replication. This guide is structured for both beginners and experienced database users looking to master PostgreSQL.

## Table of Contents 📑

1. [Introduction to PostgreSQL](#introduction-to-postgresql)
2. [Installation & Setup](#installation--setup)
3. [Basic SQL Queries](#basic-sql-queries)
4. [Data Types in PostgreSQL](#data-types-in-postgresql)
5. [Constraints & Keys](#constraints--keys)
6. [Joins & Subqueries](#joins--subqueries)
7. [Views & Materialized Views](#views--materialized-views)
8. [Indexes & Performance Tuning](#indexes--performance-tuning)
9. [Stored Procedures & Functions](#stored-procedures--functions)
10. [Triggers & Event Listeners](#triggers--event-listeners)
11. [Transactions & Concurrency Control](#transactions--concurrency-control)
12. [Backup & Restore](#backup--restore)
13. [Security & User Management](#security--user-management)
14. [JSON & PostgreSQL](#json--postgresql)
15. [Partitioning & Sharding](#partitioning--sharding)
16. [CTE (Common Table Expressions)](#cte-common-table-expressions)
17. [Full-Text Search](#full-text-search)
18. [Replication & High Availability](#replication--high-availability)
19. [PostgreSQL Extensions](#postgresql-extensions)

---

## 1. Introduction to PostgreSQL 🐘
PostgreSQL is an **open-source relational database management system (RDBMS)** known for its extensibility, reliability, and compliance with SQL standards. It supports:
- **ACID Transactions** for data integrity.
- **Advanced Indexing** for performance optimization.
- **Extensibility** with custom functions and data types.
- **Concurrency Control** ensuring data consistency.

## 2. Installation & Setup ⚙️
- **Installing PostgreSQL on Linux/macOS:**
  ```sh
  sudo apt update && sudo apt install postgresql postgresql-contrib  # Debian-based systems
  brew install postgresql  # macOS (Homebrew)
  ```
- **Starting PostgreSQL Service:**
  ```sh
  sudo systemctl start postgresql  # Linux
  brew services start postgresql   # macOS
  ```
- **Accessing PostgreSQL CLI (psql):**
  ```sh
  psql -U postgres
  ```

## 3. Basic SQL Queries 📄
Basic SQL queries are the foundation of working with PostgreSQL. Below are various essential queries for database management, CRUD operations, and advanced filtering techniques.

### **Creating a Database**
```sql
CREATE DATABASE my_database;
```
**Output:**
```
CREATE DATABASE
```

### **Connecting to a Database**
```sh
psql -U postgres -d my_database
```

### **Creating a Table**
```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    age INT CHECK (age > 0),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```
**Output:**
```
CREATE TABLE
```

### **Inserting Data**
```sql
INSERT INTO users (name, email, age) VALUES ('Alice', 'alice@example.com', 25);
INSERT INTO users (name, email, age) VALUES ('Bob', 'bob@example.com', 30);
```
**Output:**
```
INSERT 0 1
INSERT 0 1
```

### **Retrieving Data**
```sql
SELECT * FROM users;
```
**Output:**
```
 id |  name  |        email         | age |       created_at       
----+--------+----------------------+-----+------------------------
  1 | Alice  | alice@example.com    |  25 | 2024-02-16 10:00:00 UTC
  2 | Bob    | bob@example.com      |  30 | 2024-02-16 10:05:00 UTC
```

### **Filtering Data (WHERE Clause)**
```sql
SELECT * FROM users WHERE age > 25;
```
**Output:**
```
 id | name |       email       | age |       created_at       
----+------+------------------+-----+------------------------
  2 | Bob  | bob@example.com  |  30 | 2024-02-16 10:05:00 UTC
```

### **Sorting Data (ORDER BY Clause)**
```sql
SELECT * FROM users ORDER BY age DESC;
```
**Output:**
```
 id | name  |       email        | age |       created_at       
----+-------+-------------------+-----+------------------------
  2 | Bob   | bob@example.com   |  30 | 2024-02-16 10:05:00 UTC
  1 | Alice | alice@example.com |  25 | 2024-02-16 10:00:00 UTC
```

### **Limiting Results (LIMIT Clause)**
```sql
SELECT * FROM users LIMIT 1;
```
**Output:**
```
 id | name  |       email        | age |       created_at       
----+-------+-------------------+-----+------------------------
  1 | Alice | alice@example.com |  25 | 2024-02-16 10:00:00 UTC
```

### **Updating Data**
```sql
UPDATE users SET age = 28 WHERE name = 'Alice';
```
**Output:**
```
UPDATE 1
```

### **Deleting Data**
```sql
DELETE FROM users WHERE name = 'Bob';
```
**Output:**
```
DELETE 1
```

### **Counting Rows**
```sql
SELECT COUNT(*) FROM users;
```
**Output:**
```
 count 
-------
     1
```

### **Checking for Null Values**
```sql
SELECT * FROM users WHERE email IS NULL;
```
**Output:**
```
(0 rows)
```

### **Grouping Data (GROUP BY Clause)**
```sql
SELECT age, COUNT(*) FROM users GROUP BY age;
```
**Output:**
```
 age | count 
-----+-------
  28 |     1
```

### **Using Aggregate Functions**
```sql
SELECT AVG(age) FROM users;
```
**Output:**
```
 avg 
-----
 28.0
```

### **Joining Data from Multiple Tables (INNER JOIN)**
```sql
CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    user_id INT REFERENCES users(id),
    product VARCHAR(100),
    quantity INT,
    order_date DATE DEFAULT CURRENT_DATE
);
INSERT INTO orders (user_id, product, quantity) VALUES (1, 'Laptop', 1);
SELECT users.name, orders.product, orders.quantity FROM users 
INNER JOIN orders ON users.id = orders.user_id;
```
**Output:**
```
 name  | product | quantity 
-------+---------+----------
 Alice | Laptop  |        1
```

### **Using Subqueries**
```sql
SELECT name FROM users WHERE id IN (SELECT user_id FROM orders);
```
**Output:**
```
 name  
--------
 Alice
```

These examples cover essential SQL operations such as **creating databases and tables, inserting and retrieving data, filtering and sorting records, performing updates and deletions, using aggregate functions, joining tables, and utilizing subqueries** in PostgreSQL. 🚀
```sql
CREATE DATABASE my_database;
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
INSERT INTO users (name, email) VALUES ('Alice', 'alice@example.com');
SELECT * FROM users;
```
**Output:**
```
 id |  name  |        email         |       created_at       
----+--------+----------------------+------------------------
  1 | Alice  | alice@example.com    | 2024-02-16 10:00:00 UTC
```

## 4. Data Types in PostgreSQL 🏗️
- **Common Data Types:**
  - `INTEGER`, `BIGINT`, `SERIAL` (Auto-incrementing ID)
  - `VARCHAR(n)`, `TEXT` (String values)
  - `BOOLEAN` (True/False)
  - `DATE`, `TIMESTAMP` (Date and time)
  - `JSON`, `JSONB` (Storing JSON data)

## 5. Constraints & Keys 🔑
```sql
CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    user_id INTEGER REFERENCES users(id),
    order_date DATE NOT NULL
);
```

## 6. Joins & Subqueries 🔄
```sql
SELECT users.name, orders.order_date 
FROM users
INNER JOIN orders ON users.id = orders.user_id;
```
**Output:**
```
 name  | order_date  
-------+------------
 Alice | 2024-02-15
```

## 7. Views & Materialized Views 👀
```sql
CREATE VIEW active_users AS
SELECT * FROM users WHERE is_active = TRUE;
```

## 8. Indexes & Performance Tuning ⚡
```sql
CREATE INDEX idx_users_email ON users(email);
EXPLAIN ANALYZE SELECT * FROM users WHERE email = 'alice@example.com';
```
**Output:**
```
QUERY PLAN
-----------------------------------------------------------------
Index Scan using idx_users_email on users  (cost=0.15..8.37 rows=1)
```

## 9. Stored Procedures & Functions ⚙️
```sql
CREATE FUNCTION get_user_count() RETURNS INTEGER AS $$
BEGIN
    RETURN (SELECT COUNT(*) FROM users);
END;
$$ LANGUAGE plpgsql;
SELECT get_user_count();
```

## 10. Triggers & Event Listeners 🔔
```sql
CREATE TRIGGER set_timestamp
BEFORE UPDATE ON users
FOR EACH ROW
EXECUTE FUNCTION update_modified_column();
```

## 11. Transactions & Concurrency Control 🔄
```sql
BEGIN;
UPDATE users SET name = 'Updated Name' WHERE id = 1;
COMMIT;
ROLLBACK;
```

## 12. Backup & Restore 💾
```sh
pg_dump -U postgres -F c my_database > backup.sql
pg_restore -U postgres -d my_database backup.sql
```

## 13. Security & User Management 🔐
```sql
CREATE USER db_user WITH ENCRYPTED PASSWORD 'secure_password';
GRANT ALL PRIVILEGES ON DATABASE my_database TO db_user;
```

## 14. JSON & PostgreSQL 📜
```sql
CREATE TABLE api_responses (
    id SERIAL PRIMARY KEY,
    data JSONB NOT NULL
);
SELECT data->>'name' FROM api_responses;
```

## 15. Partitioning & Sharding 📌
```sql
CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    order_date DATE NOT NULL
) PARTITION BY RANGE (order_date);
```

## 16. CTE (Common Table Expressions) 📜
```sql
WITH top_customers AS (
    SELECT user_id, COUNT(*) as order_count
    FROM orders
    GROUP BY user_id
    HAVING COUNT(*) > 5
)
SELECT users.name, top_customers.order_count
FROM users
JOIN top_customers ON users.id = top_customers.user_id;
```

## 17. Full-Text Search 🔍
```sql
SELECT * FROM articles WHERE to_tsvector(title || ' ' || body) @@ to_tsquery('search_term');
```

## 18. Replication & High Availability 🔄
```sh
pg_basebackup -h master_ip -D /var/lib/postgresql/12/main -U replication -P
```

## 19. PostgreSQL Extensions 🛠️
```sql
CREATE EXTENSION IF NOT EXISTS pg_stat_statements;
```

---


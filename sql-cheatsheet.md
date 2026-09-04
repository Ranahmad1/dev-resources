# SQL Cheatsheet

## Database & Table Operations
```sql
-- Create database
CREATE DATABASE mydb;
USE mydb;

-- Create table
CREATE TABLE users (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(100) NOT NULL,
  email VARCHAR(150) UNIQUE NOT NULL,
  age INT,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Modify table
ALTER TABLE users ADD COLUMN phone VARCHAR(20);
ALTER TABLE users MODIFY COLUMN name VARCHAR(200);
ALTER TABLE users DROP COLUMN phone;
ALTER TABLE users RENAME TO customers;

-- Delete table
DROP TABLE users;
TRUNCATE TABLE users;  -- Delete all rows, keep table
```

## CRUD Operations
```sql
-- INSERT
INSERT INTO users (name, email, age) VALUES ('Ahmad', 'ahmad@email.com', 20);
INSERT INTO users (name, email) VALUES ('Ali', 'ali@email.com'), ('Sara', 'sara@email.com');

-- SELECT
SELECT * FROM users;
SELECT name, email FROM users;
SELECT DISTINCT city FROM users;
SELECT COUNT(*) AS total FROM users;
SELECT name, age FROM users LIMIT 10 OFFSET 20;

-- UPDATE
UPDATE users SET age = 21 WHERE id = 1;
UPDATE users SET age = age + 1 WHERE created_at < '2024-01-01';

-- DELETE
DELETE FROM users WHERE id = 1;
DELETE FROM users WHERE age < 18;
```

## WHERE Conditions
```sql
WHERE age = 20
WHERE age != 20  OR  WHERE age <> 20
WHERE age > 18 AND age < 60
WHERE age BETWEEN 18 AND 30
WHERE name IN ('Ahmad', 'Ali', 'Sara')
WHERE name NOT IN ('Admin')
WHERE email LIKE '%@gmail.com'
WHERE name LIKE 'A%'        -- Starts with A
WHERE phone IS NULL
WHERE phone IS NOT NULL
```

## ORDER BY & GROUP BY
```sql
-- Order
SELECT * FROM users ORDER BY name ASC;
SELECT * FROM users ORDER BY created_at DESC;

-- Group
SELECT city, COUNT(*) as total
FROM users
GROUP BY city
HAVING COUNT(*) > 5
ORDER BY total DESC;
```

## Aggregate Functions
```sql
SELECT COUNT(*) FROM users;
SELECT AVG(age) FROM users;
SELECT SUM(salary) FROM employees;
SELECT MIN(age), MAX(age) FROM users;
SELECT ROUND(AVG(score), 2) FROM results;
```

## JOINs
```sql
-- INNER JOIN (matching rows only)
SELECT users.name, orders.product
FROM users
INNER JOIN orders ON users.id = orders.user_id;

-- LEFT JOIN (all from left, matching from right)
SELECT users.name, orders.product
FROM users
LEFT JOIN orders ON users.id = orders.user_id;

-- RIGHT JOIN
SELECT * FROM orders
RIGHT JOIN users ON orders.user_id = users.id;

-- Multiple JOINs
SELECT u.name, o.product, p.amount
FROM users u
JOIN orders o ON u.id = o.user_id
JOIN payments p ON o.id = p.order_id;
```

## Subqueries
```sql
-- In WHERE
SELECT name FROM users
WHERE id IN (SELECT user_id FROM orders WHERE total > 1000);

-- In FROM
SELECT avg_age FROM (
  SELECT AVG(age) AS avg_age FROM users
) AS subquery;
```

## Indexes
```sql
CREATE INDEX idx_email ON users(email);
CREATE UNIQUE INDEX idx_username ON users(username);
DROP INDEX idx_email ON users;
SHOW INDEX FROM users;
```

## Useful Functions
```sql
NOW()                    -- Current datetime
CURDATE()                -- Current date
DATE_FORMAT(date, '%d-%m-%Y')  -- Format date
CONCAT(first, ' ', last) -- Concatenate
UPPER(name), LOWER(name) -- Change case
LENGTH(text)             -- String length
SUBSTRING(str, 1, 5)     -- Substring
COALESCE(col, 'default') -- Return first non-null
IF(age >= 18, 'Adult', 'Minor') -- Conditional
```

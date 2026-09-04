# SQL Cheatsheet

## Database & Table Operations
```sql
CREATE DATABASE mydb CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
USE mydb;
SHOW DATABASES;
SHOW TABLES;
DESCRIBE users;

-- Create table
CREATE TABLE users (
  id         INT           AUTO_INCREMENT PRIMARY KEY,
  name       VARCHAR(100)  NOT NULL,
  email      VARCHAR(150)  UNIQUE NOT NULL,
  role       ENUM('admin','user','guest') DEFAULT 'user',
  is_active  TINYINT(1)    DEFAULT 1,
  created_at TIMESTAMP     DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP     DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);

-- Modify table
ALTER TABLE users ADD    COLUMN phone VARCHAR(20) AFTER email;
ALTER TABLE users MODIFY COLUMN name  VARCHAR(200) NOT NULL;
ALTER TABLE users DROP   COLUMN phone;
ALTER TABLE users RENAME TO customers;
ALTER TABLE users ADD CONSTRAINT fk_dept FOREIGN KEY (dept_id) REFERENCES departments(id);

DROP TABLE IF EXISTS users;   -- Delete table
TRUNCATE TABLE users;          -- Delete all rows, reset auto-increment
```

## CRUD Operations
```sql
-- INSERT
INSERT INTO users (name, email) VALUES ('Ahmad', 'ahmad@email.com');
INSERT INTO users (name, email) VALUES
  ('Ali',  'ali@email.com'),
  ('Sara', 'sara@email.com');

-- INSERT OR IGNORE / ON DUPLICATE KEY UPDATE
INSERT INTO users (name, email) VALUES ('Ahmad', 'a@b.com')
ON DUPLICATE KEY UPDATE name = VALUES(name);

-- SELECT
SELECT * FROM users;
SELECT id, name, email FROM users;
SELECT DISTINCT role FROM users;
SELECT COUNT(*) AS total FROM users;
SELECT * FROM users LIMIT 10 OFFSET 20;   -- pagination: page 3 (size 10)
SELECT * FROM users ORDER BY created_at DESC LIMIT 10;

-- UPDATE
UPDATE users SET role = 'admin' WHERE id = 1;
UPDATE users SET is_active = 0 WHERE last_login < NOW() - INTERVAL 6 MONTH;

-- DELETE
DELETE FROM users WHERE id = 1;
DELETE FROM users WHERE is_active = 0 AND created_at < '2024-01-01';
```

## WHERE Conditions
```sql
WHERE age = 20
WHERE age != 20       -- or <>
WHERE age > 18 AND age < 60
WHERE age BETWEEN 18 AND 30      -- inclusive
WHERE role IN ('admin', 'editor')
WHERE role NOT IN ('guest')
WHERE name LIKE 'A%'             -- starts with A
WHERE email LIKE '%@gmail.com'   -- ends with
WHERE name LIKE '_ah%'           -- _ = any single char
WHERE phone IS NULL
WHERE phone IS NOT NULL
WHERE (role = 'admin' OR is_active = 1) AND age > 18
```

## ORDER BY, GROUP BY, HAVING
```sql
SELECT * FROM users ORDER BY name ASC, created_at DESC;

SELECT
  role,
  COUNT(*)       AS total,
  AVG(age)       AS avg_age,
  MAX(created_at) AS latest
FROM users
GROUP BY role
HAVING COUNT(*) >= 5      -- filter AFTER grouping (not WHERE)
ORDER BY total DESC;
```

## Aggregate Functions
```sql
COUNT(*)              -- all rows
COUNT(DISTINCT email) -- unique values
SUM(salary)
AVG(salary)
MIN(age)
MAX(salary)
ROUND(AVG(score), 2)  -- 2 decimal places
GROUP_CONCAT(name SEPARATOR ', ')  -- join values
```

## JOINs
```sql
-- INNER JOIN — only matching rows
SELECT u.name, o.product, o.total
FROM users u
INNER JOIN orders o ON u.id = o.user_id;

-- LEFT JOIN — all from left, NULL for no match on right
SELECT u.name, o.product
FROM users u
LEFT JOIN orders o ON u.id = o.user_id;

-- RIGHT JOIN
SELECT u.name, o.product
FROM orders o
RIGHT JOIN users u ON o.user_id = u.id;

-- FULL OUTER JOIN (MySQL workaround)
SELECT * FROM users u LEFT  JOIN orders o ON u.id = o.user_id
UNION
SELECT * FROM users u RIGHT JOIN orders o ON u.id = o.user_id;

-- SELF JOIN
SELECT e.name AS employee, m.name AS manager
FROM employees e
LEFT JOIN employees m ON e.manager_id = m.id;

-- Multiple JOINs
SELECT u.name, o.product, p.status
FROM users u
JOIN orders  o ON u.id = o.user_id
JOIN payments p ON o.id = p.order_id
WHERE p.status = 'completed';
```

## Subqueries
```sql
-- In WHERE
SELECT name FROM users
WHERE id IN (SELECT user_id FROM orders WHERE total > 1000);

-- Correlated subquery
SELECT name FROM users u
WHERE EXISTS (
  SELECT 1 FROM orders o WHERE o.user_id = u.id
);

-- In FROM (derived table)
SELECT dept, avg_salary FROM (
  SELECT dept_id AS dept, AVG(salary) AS avg_salary
  FROM employees GROUP BY dept_id
) AS dept_stats
WHERE avg_salary > 50000;

-- CTE (Common Table Expression) — cleaner subquery
WITH active_users AS (
  SELECT * FROM users WHERE is_active = 1
),
recent_orders AS (
  SELECT * FROM orders WHERE created_at > NOW() - INTERVAL 30 DAY
)
SELECT u.name, o.total
FROM active_users u
JOIN recent_orders o ON u.id = o.user_id;
```

## Indexes
```sql
CREATE INDEX idx_email           ON users(email);
CREATE UNIQUE INDEX idx_username ON users(username);
CREATE INDEX idx_compound        ON orders(user_id, created_at);
DROP INDEX idx_email             ON users;
SHOW INDEX FROM users;
EXPLAIN SELECT * FROM users WHERE email = 'a@b.com';  -- check query plan
```

## Useful Functions
```sql
-- Date/Time
NOW()                              -- current datetime
CURDATE(), CURTIME()
DATE_FORMAT(created_at, '%d-%m-%Y')
DATEDIFF(end_date, start_date)     -- days between
DATE_ADD(NOW(), INTERVAL 7 DAY)
YEAR(created_at), MONTH(), DAY()

-- String
CONCAT(first_name, ' ', last_name)
CONCAT_WS(' ', first_name, last_name)  -- with separator
UPPER(name), LOWER(email)
LENGTH(text)      -- bytes | CHAR_LENGTH for characters
SUBSTRING(str, 1, 5)
TRIM(str)
REPLACE(str, 'old', 'new')

-- Conditional
IF(age >= 18, 'Adult', 'Minor')
CASE
  WHEN role = 'admin' THEN 'Administrator'
  WHEN role = 'user'  THEN 'Regular User'
  ELSE 'Guest'
END AS role_label
COALESCE(phone, mobile, 'N/A')    -- first non-null
NULLIF(value, 0)                   -- return NULL if equal

-- Math
ROUND(price, 2)
CEIL(4.1)  -- 5
FLOOR(4.9) -- 4
ABS(-5)    -- 5
MOD(10, 3) -- 1
RAND()     -- random 0-1
```

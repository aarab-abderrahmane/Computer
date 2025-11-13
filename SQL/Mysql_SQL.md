
# **Complete Expanded Explanation — MySQL SQL + MySQL Procedural Programming**

MySQL supports two layers of programming:

1. **Standard SQL** → used for creating tables, inserting data, updating data, etc.
2. **MySQL Procedural Language (SQL/PSM)** → adds programming logic (variables, loops, conditions, procedures, functions, triggers, events).

The procedural extensions allow MySQL to behave slightly like a programming language.

---

# **1. Standard SQL Foundations (DDL & DML)**

Before learning procedural programming, you must fully understand **standard SQL**, because all procedural code relies on standard SQL commands.

---

## **1.1 DDL — Data Definition Language**

Used to define the *structure* of databases and tables.

| Operation       | Meaning            | Example                                      |
| --------------- | ------------------ | -------------------------------------------- |
| Create database | Create a new DB    | `CREATE DATABASE shop_db;`                   |
| Delete database | Remove a DB        | `DROP DATABASE shop_db;`                     |
| Create table    | Define a new table | `CREATE TABLE products (...);`               |
| Modify table    | Add/drop columns   | `ALTER TABLE products ADD description TEXT;` |
| Delete table    | Remove a table     | `DROP TABLE products;`                       |

**Example of a full table definition:**

```sql
CREATE TABLE products (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    price DECIMAL(10,2),
    stock INT DEFAULT 0
) ENGINE=InnoDB;
```

---

## **1.2 DML — Data Manipulation Language**

Used to manipulate data *inside* a table.

| Operation | Meaning      | Example                                               |
| --------- | ------------ | ----------------------------------------------------- |
| INSERT    | Add new rows | `INSERT INTO products (...) VALUES (...);`            |
| UPDATE    | Update rows  | `UPDATE products SET price = 200 WHERE id = 1;`       |
| DELETE    | Remove rows  | `DELETE FROM products WHERE id = 1;`                  |
| SELECT    | Query data   | `SELECT name, price FROM products WHERE price > 100;` |

### **Aggregation Functions**

* `COUNT(*)` → number of rows
* `AVG(price)` → average
* `MAX(price)` → maximum
* `SUM(price)` → sum

Example:

```sql
SELECT COUNT(*), AVG(price), MAX(price) FROM products;
```

### **Joins (INNER, LEFT, RIGHT)**

Used to combine data from multiple tables.

---

# **2. Procedural Programming in MySQL (SQL/PSM)**

Standard SQL has **no conditions or loops**.
MySQL extends SQL to add programming logic.

This layer allows MySQL to execute "programs" on the server side, which means:

* automatic tasks
* reusable code
* complex logic (IF, WHILE, CASE…)
* handling errors
* looping through rows

---

# **2.1 Block Structure (BEGIN / END)**

Every procedural program lives inside:

```
BEGIN
    -- declarations
    -- instructions
END;
```

A block may contain:

1. **DECLARE statements** (optional)
2. **Variables**
3. **Conditions (IF, CASE)**
4. **Loops (WHILE, REPEAT, LOOP)**
5. **SQL statements (SELECT, INSERT, UPDATE)**

### **Important: Variable Scope**

* A variable declared in a block is visible inside that block and all nested blocks.
* A variable declared inside a nested block is *not* visible outside.

---

# **2.2 Declaring Variables**

MySQL supports two categories:

---

### **1. Local Variables (Scalars)**

Declared using `DECLARE` inside BEGIN/END:

```sql
DECLARE age INT;
DECLARE birthdate DATE;
DECLARE total, counter INT;
```

* Exist **only inside the block**
* Reset every time the block runs

---

### **2. Session Variables (@variables)**

Start with `@`:

```sql
SET @x = 10;
SELECT @max_price := MAX(price) FROM products;
```

Properties:

* They live **for the duration of the session** (your connection to MySQL).
* Other users cannot see them.
* They are not declared with `DECLARE`.

Used mainly for debugging, testing, or simple scripts.

---

# **3. Main Types of MySQL Stored Programs**

MySQL procedural programming has four main program types:

---

## **3.1 Stored Procedures**

* A stored procedure is a block of reusable SQL.
* It can accept input parameters (`IN`), output parameters (`OUT`), or both.
* It **does not return a single value** like functions do.
* It can return *tables* (via SELECT queries).
* It can perform INSERT/UPDATE/DELETE/etc.

**Example:**

```sql
DELIMITER $$

CREATE PROCEDURE sp_clients_by_city(IN city VARCHAR(50))
BEGIN
    SELECT *
    FROM clients
    WHERE UPPER(address) = UPPER(city);
END $$

DELIMITER ;
```

Call it:

```sql
CALL sp_clients_by_city('Agadir');
```

---

## **3.2 Stored Functions**

* Always return **exactly one value**.
* Cannot return a result set (no SELECT table output).
* Can be used *inside* SQL statements.

Example: Count clients in a city.

```sql
DELIMITER $$

CREATE FUNCTION fn_count_clients(city VARCHAR(50))
RETURNS INT
READS SQL DATA
BEGIN
    DECLARE total INT;
    SELECT COUNT(*) INTO total FROM clients WHERE address = city;
    RETURN total;
END $$

DELIMITER ;
```

Call it:

```sql
SELECT fn_count_clients('Agadir');
```

---

# **3.3 Triggers**

A trigger is executed **automatically** when a specific event happens on a table:

* BEFORE INSERT
* AFTER INSERT
* BEFORE UPDATE
* AFTER UPDATE
* BEFORE DELETE
* AFTER DELETE

Example: Prevent inserting clients younger than 18:

```sql
DELIMITER //

CREATE TRIGGER tr_check_age BEFORE INSERT ON clients
FOR EACH ROW
BEGIN
    DECLARE minAge INT DEFAULT 18;

    IF NEW.birth_date > DATE_SUB(CURDATE(), INTERVAL minAge YEAR) THEN
        SIGNAL SQLSTATE '50001' 
        SET MESSAGE_TEXT = 'Client must be 18 or older.';
    END IF;

END //

DELIMITER ;
```

Notes:

* `NEW.field` → new value being inserted or updated
* `OLD.field` → value before update/delete

---

# **3.4 Events (Scheduled Tasks)**

Events are automated tasks executed at specific times (like cron jobs).

Use cases:

* nightly backups
* clearing old logs
* periodic inserts or updates

Example: Insert a row every minute for one hour:

```sql
CREATE EVENT test_event
ON SCHEDULE EVERY 1 MINUTE
STARTS CURRENT_TIMESTAMP
ENDS CURRENT_TIMESTAMP + INTERVAL 1 HOUR
DO
INSERT INTO messages (message, created_at)
VALUES ('Test event', NOW());
```

---

# **4. Control Structures (IF, CASE, LOOPS)**

Procedural MySQL adds control flow similar to real programming languages.

---

## **4.1 Conditionals**

### IF / ELSE

```sql
IF salary > 5000 THEN
    SET category = 'High';
ELSE
    SET category = 'Normal';
END IF;
```

### IF / ELSEIF / ELSE

```sql
IF x < 10 THEN
    SET level = 'Low';
ELSEIF x < 20 THEN
    SET level = 'Medium';
ELSE
    SET level = 'High';
END IF;
```

### CASE

```sql
CASE country
    WHEN 'USA' THEN SET shipping = '2 days';
    WHEN 'France' THEN SET shipping = '3 days';
    ELSE SET shipping = '5 days';
END CASE;
```

---

# **4.2 Loops**

MySQL supports 3 types of loops.

---

### **1. LOOP**

Runs forever unless you manually exit.

```sql
simple_loop: LOOP
    SET counter = counter + 1;

    IF counter > 10 THEN
        LEAVE simple_loop;
    END IF;

END LOOP simple_loop;
```

* `LEAVE` = break
* `ITERATE` = continue

---

### **2. WHILE Loop**

```sql
WHILE x > 0 DO
    SET x = x - 1;
END WHILE;
```

Runs only **while the condition is true**.

---

### **3. REPEAT Loop**

Runs **at least once**, stops when condition becomes true.

```sql
REPEAT
    SET x = x + 1;
UNTIL x > limit
END REPEAT;
```

---

# **5. Advanced Concepts**

---

## **5.1 Transactions**

A transaction ensures **all operations succeed together**, or none at all.

Atomic operation: “All or nothing.”

Used in bank transfers, reservations, inventory deduction, etc.

Structure:

```sql
START TRANSACTION;

UPDATE accounts SET balance = balance - 500 WHERE id = 1;
UPDATE accounts SET balance = balance + 500 WHERE id = 2;

COMMIT; -- Save changes
```

If something fails:

```sql
ROLLBACK; -- Cancel all changes
```

---

## **5.2 Exception Handling**

Used to handle errors inside procedures.

Example:

```sql
DECLARE EXIT HANDLER FOR 1048
SELECT 'Name cannot be NULL';
```

* `EXIT` stops the block after error
* `CONTINUE` keeps running

---

## **5.3 Cursors**

A cursor lets you process a SELECT result **row by row**, like iterating through a list.

Steps:

1. Declare cursor
2. Open cursor
3. Fetch each row
4. Close cursor
5. Use `NOT FOUND` handler to detect end of data

Example:

```sql
DECLARE finished INT DEFAULT 0;
DECLARE cur CURSOR FOR SELECT id, name FROM clients;
DECLARE CONTINUE HANDLER FOR NOT FOUND SET finished = 1;

OPEN cur;

read_loop: LOOP
    FETCH cur INTO v_id, v_name;

    IF finished = 1 THEN
        LEAVE read_loop;
    END IF;

    -- process data here
END LOOP;

CLOSE cur;
```



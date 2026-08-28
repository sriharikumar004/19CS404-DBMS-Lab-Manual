# Experiment 10: PL/SQL – Triggers

## AIM
To write and execute PL/SQL trigger programs for automating actions in response to specific table events like INSERT, UPDATE, or DELETE.

---

## THEORY

A **trigger** is a stored PL/SQL block that is automatically executed or fired when a specified event occurs on a table or view. Triggers can be used for enforcing business rules, auditing changes, or automatic updates.

### Types of Triggers:
- **Before Trigger**: Executes before the operation (INSERT, UPDATE, DELETE).
- **After Trigger**: Executes after the operation.
- **Row-level Trigger**: Executes for each affected row.
- **Statement-level Trigger**: Executes once for the triggering statement.

**Basic Syntax:**
```sql
CREATE OR REPLACE TRIGGER trigger_name
BEFORE|AFTER INSERT|UPDATE|DELETE ON table_name
[FOR EACH ROW]
BEGIN
   -- trigger logic
END;
```

## 1. Write a trigger to log every insertion into a table.
**Steps:**
- Create two tables: `employees` (for storing data) and `employee_log` (for logging the inserts).
- Write an **AFTER INSERT** trigger on the `employees` table to log the new data into the `employee_log` table.

-**Program**
 ```
  SET SERVEROUTPUT ON;
CREATE TABLE student (
    student_id NUMBER,
    student_name VARCHAR2(50),
    department VARCHAR2(50)
);
CREATE TABLE student_log (
    student_id NUMBER,
    student_name VARCHAR2(50),
    action VARCHAR2(20)
);
CREATE OR REPLACE TRIGGER student_insert_log
AFTER INSERT ON student
FOR EACH ROW
BEGIN
    INSERT INTO student_log
    VALUES (:NEW.student_id, :NEW.student_name, 'INSERT');
END;
INSERT INTO student
VALUES (1, 'Hari', 'Computer Science');
SELECT * FROM student_log;
```

**Expected Output:**
- A new entry is added to the `employee_log` table each time a new record is inserted into the `employees` table.

- <img width="893" height="715" alt="image" src="https://github.com/user-attachments/assets/dcf9ff04-2486-4d10-a3b3-12a491878b36" />


---

## 2. Write a trigger to prevent deletion of records from a sensitive table.
**Steps:**
- Write a **BEFORE DELETE** trigger on the `sensitive_data` table.
- Use `RAISE_APPLICATION_ERROR` to prevent deletion and issue a custom error message.

**Program**
```
CREATE TABLE sensitive_data (
    id NUMBER,
    data VARCHAR2(100)
);
INSERT INTO sensitive_data VALUES (1, 'Confidential Data');
CREATE OR REPLACE TRIGGER prevent_delete
BEFORE DELETE ON sensitive_data
FOR EACH ROW
BEGIN
    RAISE_APPLICATION_ERROR(-20001,
        'Deletion not allowed on this table.');
END;
DELETE FROM sensitive_data WHERE id = 1;
```

**Expected Output:**
- If an attempt is made to delete a record from `sensitive_data`, an error message is raised, e.g., `ERROR: Deletion not allowed on this table.`

<img width="806" height="761" alt="image" src="https://github.com/user-attachments/assets/11551c07-e622-4779-872e-53a3ac82b727" />

---

## 3. Write a trigger to automatically update a `last_modified` timestamp.
**Steps:**
- Add a `last_modified` column to the `products` table.
- Write a **BEFORE UPDATE** trigger on the `products` table to set the `last_modified` column to the current timestamp whenever an update occurs.

**Program**
```
CREATE TABLE products (
    product_id NUMBER,
    product_name VARCHAR2(50),
    price NUMBER,
    last_modified TIMESTAMP
);
INSERT INTO products
VALUES (1, 'Laptop', 50000, SYSTIMESTAMP);
CREATE OR REPLACE TRIGGER update_last_modified
BEFORE UPDATE ON products
FOR EACH ROW
BEGIN
    :NEW.last_modified := SYSTIMESTAMP;
END;
UPDATE products
SET price = 55000
WHERE product_id = 1;
SELECT * FROM products;
```
**Expected Output:**
- The `last_modified` column in the `products` table is updated automatically to the current date and time when any record is updated.

<img width="949" height="711" alt="image" src="https://github.com/user-attachments/assets/69e561ea-7a68-4d73-9e7d-c6429f98913f" />

---

## 4. Write a trigger to keep track of the number of updates made to a table.
**Steps:**
- Create an `audit_log` table with a counter column.
- Write an **AFTER UPDATE** trigger on the `customer_orders` table to increment the counter in the `audit_log` table every time a record is updated.

**Program**
```
CREATE TABLE customer_orders (
    order_id NUMBER,
    customer_name VARCHAR2(50),
    amount NUMBER
);
CREATE TABLE audit_log (
    update_count NUMBER
);
INSERT INTO audit_log VALUES (0);
INSERT INTO customer_orders
VALUES (1, 'Ajay', 5000);
CREATE OR REPLACE TRIGGER count_updates
AFTER UPDATE ON customer_orders
FOR EACH ROW
BEGIN
    UPDATE audit_log
    SET update_count = update_count + 1;
END;
UPDATE customer_orders
SET amount = 6000
WHERE order_id = 1;
SELECT * FROM audit_log;
```
**Expected Output:**
- The `audit_log` table will maintain a count of how many updates have been made to the `customer_orders` table.
- 
<img width="598" height="692" alt="image" src="https://github.com/user-attachments/assets/952f3d34-2726-442f-ae24-ea056d31516c" />

---

## 5. Write a trigger that checks a condition before allowing insertion into a table.
**Steps:**
- Write a **BEFORE INSERT** trigger on the `employees` table to check if the inserted salary meets a specific condition (e.g., salary must be greater than 3000).
- If the condition is not met, raise an error to prevent the insert.

- **Program**
 ```
  CREATE TABLE customer_accounts (
    account_id NUMBER,
    customer_name VARCHAR2(50),
    balance NUMBER
);
CREATE OR REPLACE TRIGGER check_min_balance
BEFORE INSERT ON customer_accounts
FOR EACH ROW
BEGIN
    IF :NEW.balance < 1000 THEN
        RAISE_APPLICATION_ERROR(
            -20004,
            'Balance is below minimum threshold.'
        );
    END IF;
END;
INSERT INTO customer_accounts
VALUES (101, 'Ravi', 5000);
SELECT * FROM customer_accounts;
```

**Expected Output:**
- If the inserted salary in the `employees` table is below the condition (e.g., salary < 3000), the insert operation is blocked, and an error message is raised, such as: `ERROR: Salary below minimum threshold.`

<img width="805" height="693" alt="image" src="https://github.com/user-attachments/assets/2eb8e5c5-7d5b-485b-92ef-0e64eb04e2a2" />

## RESULT
Thus, the PL/SQL trigger programs were written and executed successfully.

# Experiment 6: Joins

## AIM
To study and implement different types of joins.

## THEORY

SQL Joins are used to combine records from two or more tables based on a related column.

### 1. INNER JOIN
Returns records with matching values in both tables.

**Syntax:**
```sql
SELECT columns
FROM table1
INNER JOIN table2
ON table1.column = table2.column;
```

### 2. LEFT JOIN
Returns all records from the left table, and matched records from the right.

**Syntax:**

```sql
SELECT columns
FROM table1
LEFT JOIN table2
ON table1.column = table2.column;
```
### 3. RIGHT JOIN
Returns all records from the right table, and matched records from the left.

**Syntax:**

```sql
SELECT columns
FROM table1
RIGHT JOIN table2
ON table1.column = table2.column;
```
### 4. FULL OUTER JOIN
Returns all records when there is a match in either left or right table.

**Syntax:**

```sql
SELECT columns
FROM table1
FULL OUTER JOIN table2
ON table1.column = table2.column;
```

**Question 1**
--
Write the SQL query that achieves the selection of all columns from the "nurses" table (aliased as "n"), with an inner join on the "department_id" column and a condition filtering for nurses in the 'Pediatrics' department.

NURSES TABLE:
ATTRIBUTES - nurse_id, first_name, last_name, department_id
DEPARTMENTS TABLE:

ATTRIBUTES - department_id, department_name
For example:
Result
nurse_id         first_name       last_name        department_id
---------------  ---------------  ---------------  ---------------
3                Sophia           Clark            3

```sql
SELECT n.* FROM nurses AS n INNER JOIN departments AS d ON n.department_id = d.department_id WHERE d.department_name = 'Pediatrics';
```

**Output:**

<img width="1224" height="432" alt="image" src="https://github.com/user-attachments/assets/a661049c-7e97-463b-aad2-f406e45d1164" />

**Question 2**
---
From the following tables write a SQL query to locate those salespeople who do not live in the same city where their customers live and have received a commission of more than 12% from the company. Return Customer Name, customer city, Salesman, salesman city, commission.  

Sample table: customer

 customer_id |   cust_name    |    city    | grade | salesman_id 
-------------+----------------+------------+-------+-------------
        3002 | Nick Rimando   | New York   |   100 |        5001
        3007 | Brad Davis     | New York   |   200 |        5001
        3005 | Graham Zusi    | California |   200 |        5002
        3008 | Julian Green   | London     |   300 |        5002
        3004 | Fabian Johnson | Paris      |   300 |        5006
        3009 | Geoff Cameron  | Berlin     |   100 |        5003
        3003 | Jozy Altidor   | Moscow     |   200 |        5007
        3001 | Brad Guzan     | London     |       |        5005
Sample table: salesman

 salesman_id |    name    |   city   | commission 
-------------+------------+----------+------------
        5001 | James Hoog | New York |       0.15
        5002 | Nail Knite | Paris    |       0.13
        5005 | Pit Alex   | London   |       0.11
        5006 | Mc Lyon    | Paris    |       0.14
        5007 | Paul Adam  | Rome     |       0.13
        5003 | Lauson Hen | San Jose |       0.12
```sql
SELECT 
    c.cust_name AS "Customer Name",
    c.city AS "city",
    s.name AS "Salesman",
    s.city AS "city",
    s.commission
FROM customer AS c
INNER JOIN salesman AS s
    ON c.salesman_id = s.salesman_id
WHERE c.city <> s.city
  AND s.commission > 0.12;
```

**Output:**

<img width="1279" height="631" alt="image" src="https://github.com/user-attachments/assets/d6411882-91bd-49d8-b6e6-8cd345b7faaa" />

**Question 3**
---
From the following tables write a SQL query to find the details of an order. Return ord_no, ord_date, purch_amt, Customer Name, grade, Salesman, commission. 

Sample table: orders

ord_no      purch_amt   ord_date    customer_id  salesman_id
----------  ----------  ----------  -----------  -----------
70001       150.5       2012-10-05  3005         5002
70009       270.65      2012-09-10  3001         5005
70002       65.26       2012-10-05  3002         5001
70004       110.5       2012-08-17  3009         5003
70007       948.5       2012-09-10  3005         5002
70005       2400.6      2012-07-27  3007         5001
70008       5760        2012-09-10  3002         5001
70010       1983.43     2012-10-10  3004         5006
70003       2480.4      2012-10-10  3009         5003
70012       250.45      2012-06-27  3008         5002
70011       75.29       2012-08-17  3003         5007
70013       3045.6      2012-04-25  3002         5001

```sql
SELECT 
    o.ord_no,
    o.ord_date,
    o.purch_amt,
    c.cust_name AS "Customer Name",
    c.grade,
    s.name AS "Salesman",
    s.commission
FROM orders AS o
INNER JOIN customer AS c
    ON o.customer_id = c.customer_id
INNER JOIN salesman AS s
    ON o.salesman_id = s.salesman_id;
```

**Output:**

<img width="1323" height="334" alt="image" src="https://github.com/user-attachments/assets/6b86628a-1c54-487f-b4bb-88cde74f537b" />

**Question 4**
---
 From the following tables write a SQL query to find salespeople who received commissions of more than 12 percent from the company. Return Customer Name, customer city, Salesman, commission.  

Sample table: customer

 customer_id |   cust_name    |    city    | grade | salesman_id 
-------------+----------------+------------+-------+-------------
        3002 | Nick Rimando   | New York   |   100 |        5001
        3007 | Brad Davis     | New York   |   200 |        5001
        3005 | Graham Zusi    | California |   200 |        5002
        3008 | Julian Green   | London     |   300 |        5002
        3004 | Fabian Johnson | Paris      |   300 |        5006
        3009 | Geoff Cameron  | Berlin     |   100 |        5003
        3003 | Jozy Altidor   | Moscow     |   200 |        5007
        3001 | Brad Guzan     | London     |       |        5005
```sql
SELECT 
    c.cust_name AS "Customer Name",
    c.city,
    s.name AS "Salesman",
    s.commission
FROM customer AS c
INNER JOIN salesman AS s
    ON c.salesman_id = s.salesman_id
WHERE s.commission > 0.12;
```

**Output:**

<img width="1048" height="227" alt="image" src="https://github.com/user-attachments/assets/59f1b6c0-1cc9-4bc6-ab6c-9950a3981b7c" />

**Question 5**
---
Write the SQL query that achieves the selection of the first name from the "patients" table (aliased as "patient_name") and all columns from the "test_results" table (aliased as "t"), with an inner join on the "patient_id" column and a condition filtering for test results with the test name 'Blood Pressure'.
PATIENTS TABLE:
ATTRIBUTES - patient_id, first_name, last_name, date_of_birth, admission_date, discharge_date, doctor_id
TEST_RESULT TABLES:
ATTRIBUTES - result_id, patient_id, test_name, result, test_date
For example:
Result
patient_name     result_id        patient_id       test_name        result      test_date
---------------  ---------------  ---------------  ---------------  ----------  ----------
Alice            1                1                Blood Pressure   120/80      2024-01-20

```sql
SELECT 
    p.first_name AS patient_name,
    t.*
FROM patients AS p
INNER JOIN test_results AS t
    ON p.patient_id = t.patient_id
WHERE t.test_name = 'Blood Pressure';
```

**Output:**

<img width="1297" height="405" alt="image" src="https://github.com/user-attachments/assets/ca9358c3-d0c4-4540-b170-b262f0550a85" />

**Question 6**
---
From the following tables write a SQL query to find those orders where the order amount exists between 500 and 2000. Return ord_no, purch_amt, cust_name, city.

Sample table: customer

 customer_id |   cust_name    |    city    | grade | salesman_id 
-------------+----------------+------------+-------+-------------
        3002 | Nick Rimando   | New York   |   100 |        5001
        3007 | Brad Davis     | New York   |   200 |        5001
        3005 | Graham Zusi    | California |   200 |        5002
        3008 | Julian Green   | London     |   300 |        5002
        3004 | Fabian Johnson | Paris      |   300 |        5006
        3009 | Geoff Cameron  | Berlin     |   100 |        5003
        3003 | Jozy Altidor   | Moscow     |   200 |        5007
        3001 | Brad Guzan     | London     |       |        5005
```sql
SELECT 
    o.ord_no,
    o.purch_amt,
    c.cust_name,
    c.city
FROM orders AS o
INNER JOIN customer AS c
    ON o.customer_id = c.customer_id
WHERE o.purch_amt BETWEEN 500 AND 2000;
```

**Output:**

<img width="1038" height="418" alt="image" src="https://github.com/user-attachments/assets/3c400985-8cad-428a-9bdd-ab0012cb94ce" />

**Question 7**
---
Write a SQL statement to join the tables salesman, customer and orders so that the same column of each table appears once and only the relational rows are returned. 

Sample table: orders

ord_no      purch_amt   ord_date    customer_id  salesman_id
----------  ----------  ----------  -----------  -----------
70001       150.5       2012-10-05  3005         5002
70009       270.65      2012-09-10  3001         5005
70002       65.26       2012-10-05  3002         5001
70004       110.5       2012-08-17  3009         5003
70007       948.5       2012-09-10  3005         5002
70005       2400.6      2012-07-27  3007         5001
70008       5760        2012-09-10  3002         5001
70010       1983.43     2012-10-10  3004         5006
70003       2480.4      2012-10-10  3009         5003
70012       250.45      2012-06-27  3008         5002
70011       75.29       2012-08-17  3003         5007
70013       3045.6      2012-04-25  3002         5001
```sql
SELECT 
    o.ord_no,
    o.purch_amt,
    o.ord_date,
    c.cust_name,
    c.city AS customer_city,
    c.grade,
    s.name AS salesman_name,
    s.city AS salesman_city,
    s.commission
FROM orders AS o
INNER JOIN customer AS c
    ON o.customer_id = c.customer_id
INNER JOIN salesman AS s
    ON o.salesman_id = s.salesman_id;
```

**Output:**

<img width="1679" height="351" alt="image" src="https://github.com/user-attachments/assets/af099c16-75cb-49a3-a7a3-886fb36651bd" />

**Question 8**
---
Write the SQL query that achieves the selection of all columns from the "nurses" table (aliased as "n") and the "department_name" column from the "departments" table, with an inner join on the "department_id" column.
NURSES TABLE:
ATTRIBUTES - nurse_id, first_name, last_name, department_id
DEPARTMENTS TABLE:
ATTRIBUTES - department_id, department_name
For example:
Result
nurse_id         first_name       last_name        department_id    department_name
---------------  ---------------  ---------------  ---------------  ---------------
1                Emma             Taylor           1                Cardiology
2                David            Moore            2                Orthopedics
3                Sophia           Clark            3                Pediatrics

```sql
SELECT n.*,d.department_name FROM nurses AS n INNER JOIN departments AS d ON n.department_id = d.department_id;
```

**Output:**

<img width="1362" height="506" alt="image" src="https://github.com/user-attachments/assets/80b06ec8-33ad-4923-a2c4-5da13a910978" />

**Question 9**
---
Write the SQL query that achieves the selection of all columns from the "salesman" table (aliased as "s"), with a left join on the "salesman_id" column and a condition filtering for customers with the name 'Fabian Johns'.

Customer Table: (customer_id, cust_name, city, grade, salesman_id)

Salesman Table: (salesman_id, name, city, commission)

For example:

Result
salesman_id      name             city             commission
---------------  ---------------  ---------------  ---------------
5006             Mc Lyon          Paris            0.14

```sql
SELECT s.* FROM salesman AS s LEFT JOIN customer AS c ON s.salesman_id = c.salesman_id WHERE c.cust_name = 'Fabian Johns';
```

**Output:**

<img width="1138" height="379" alt="image" src="https://github.com/user-attachments/assets/a8140ae1-c76f-4db0-b9d5-021d9507751e" />

**Question 10**
---
Write the SQL query that achieves the selection of the "name" column from the "salesman" table (aliased as "s"), with a left join on the "salesman_id" column and a condition filtering for customers in the city 'New York'.

Customer Table: (customer_id, cust_name, city, grade, salesman_id)

Salesman Table: (salesman_id, name, city, commission)

For example:

Result
name
Bob Emily

```sql
SELECT s.name FROM salesman AS s LEFT JOIN customer AS c ON s.salesman_id = c.salesman_id WHERE c.city = 'New York';
```

**Output:**

<img width="499" height="395" alt="image" src="https://github.com/user-attachments/assets/63043897-52d5-43c4-8294-a39e4c7a878c" />


## RESULT
Thus, the SQL queries to implement different types of joins have been executed successfully.

# Experiment 2: DDL Commands

## AIM
To study and implement DDL commands and different types of constraints.

## THEORY

### 1. CREATE
Used to create a new relation (table).

**Syntax:**
```sql
CREATE TABLE (
  field_1 data_type(size),
  field_2 data_type(size),
  ...
);
```
### 2. ALTER
Used to add, modify, drop, or rename fields in an existing relation.
(a) ADD
```sql
ALTER TABLE std ADD (Address CHAR(10));
```
(b) MODIFY
```sql
ALTER TABLE relation_name MODIFY (field_1 new_data_type(size));
```
(c) DROP
```sql
ALTER TABLE relation_name DROP COLUMN field_name;
```
(d) RENAME
```sql
ALTER TABLE relation_name RENAME COLUMN old_field_name TO new_field_name;
```
### 3. DROP TABLE
Used to permanently delete the structure and data of a table.
```sql
DROP TABLE relation_name;
```
### 4. RENAME
Used to rename an existing database object.
```sql
RENAME TABLE old_relation_name TO new_relation_name;
```
### CONSTRAINTS
Constraints are used to specify rules for the data in a table. If there is any violation between the constraint and the data action, the action is aborted by the constraint. It can be specified when the table is created (using CREATE TABLE) or after it is created (using ALTER TABLE).
### 1. NOT NULL
When a column is defined as NOT NULL, it becomes mandatory to enter a value in that column.
Syntax:
```sql
CREATE TABLE Table_Name (
  column_name data_type(size) NOT NULL
);
```
### 2. UNIQUE
Ensures that values in a column are unique.
Syntax:
```sql
CREATE TABLE Table_Name (
  column_name data_type(size) UNIQUE
);
```
### 3. CHECK
Specifies a condition that each row must satisfy.
Syntax:
```sql
CREATE TABLE Table_Name (
  column_name data_type(size) CHECK (logical_expression)
);
```
### 4. PRIMARY KEY
Used to uniquely identify each record in a table.
Properties:
Must contain unique values.
Cannot be null.
Should contain minimal fields.
Syntax:
```sql
CREATE TABLE Table_Name (
  column_name data_type(size) PRIMARY KEY
);
```
### 5. FOREIGN KEY
Used to reference the primary key of another table.
Syntax:
```sql
CREATE TABLE Table_Name (
  column_name data_type(size),
  FOREIGN KEY (column_name) REFERENCES other_table(column)
);
```
### 6. DEFAULT
Used to insert a default value into a column if no value is specified.

Syntax:
```sql
CREATE TABLE Table_Name (
  col_name1 data_type,
  col_name2 data_type,
  col_name3 data_type DEFAULT 'default_value'
);
```

**Question 1**
--
Create a table named Employees with the following columns:

EmployeeID as INTEGER
FirstName as TEXT
LastName as TEXT
HireDate as DATE
For example:

Test	Result
pragma table_info('Employees');
cid   name        type        notnull     dflt_value  pk
----  ----------  ----------  ----------  ----------  ----------
0     EmployeeID  INTEGER     0                       0
1     FirstName   TEXT        0                       0
2     LastName    TEXT        0                       0
3     HireDate    DATE        0                       0


```
CREATE TABLE Employees(
EmployeeID  INTEGER,
FirstName TEXT,
LastName TEXT,
HireDate DATE 
)
```

**Output:**

<img width="1236" height="301" alt="image" src="https://github.com/user-attachments/assets/57f68d38-9f28-44d2-8703-c55a6768b863" />

**Question 2**
---
Create a table named Invoices with the following constraints:

InvoiceID as INTEGER should be the primary key.
InvoiceDate as DATE.
DueDate as DATE should be greater than the InvoiceDate.
Amount as REAL should be greater than 0.
For example:

Test	Result
INSERT INTO Invoices (InvoiceID, InvoiceDate)
VALUES (1, '2024-08-08'),(1,'2024-09-08');
Error: UNIQUE constraint failed: Invoices.InvoiceID

```
CREATE TABLE Invoices(
InvoiceID INTEGER primary key,
InvoiceDate DATE,
DueDate DATE CHECK(DueDate>InvoiceDate),
Amount REAL CHECK(Amount>0) 
);
```

**Output:**

<img width="1185" height="271" alt="image" src="https://github.com/user-attachments/assets/6b0c8f4a-0662-4a78-ac30-c0cbb9b83ee5" />

**Question 3**
---
Create a table named Employees with the following constraints:

EmployeeID should be the primary key.
FirstName and LastName should be NOT NULL.
Email should be unique.
Salary should be greater than 0.
DepartmentID should be a foreign key referencing the Departments table.
For example:

Test	Result
-- Attempt to insert a record with NULL FirstName
INSERT INTO Employees (EmployeeID, FirstName, LastName, Email, Salary, DepartmentID)
VALUES (1, NULL, 'Doe', 'john.doe@example.com', 50000, 1);
Error: NOT NULL constraint failed: Employees.FirstName

```sql
CREATE TABLE Employees(
EmployeeID primary key,
FirstName VARCHAR(30) NOT NULL,
LastName VARCHAR(30) NOT NULL,
Email VARCHAR(30) UNIQUE,
Salary INTEGER CHECK(Salary>0),
DepartmentID INT,
foreign key(DepartmentID) REFERENCES Departments(DepartmentID)
);
```

**Output:**

<img width="1260" height="324" alt="image" src="https://github.com/user-attachments/assets/26fd78a9-c33f-472b-8dfd-c8a96430bf93" />

**Question 4**
---
Create a table named Products with the following constraints:

ProductID should be the primary key.
ProductName should be NOT NULL.
Price is of real datatype and should be greater than 0.
Stock is of integer datatype and should be greater than or equal to 0.
For example:

Test	Result
INSERT INTO Products
VALUES (1, NULL,0,5);
Error: NOT NULL constraint failed: Products.ProductName

```CREATE TABLE Products(
ProductID primary key,
ProductName NOT NULL,
Price real CHECK(Price>0),
Stock integer CHECK(Stock>0) 
);
```

**Output:**

<img width="1050" height="238" alt="image" src="https://github.com/user-attachments/assets/a8a583cc-a147-436f-9980-5c8647043ec7" />

**Question 5**
---
Insert the below data into the Books table, allowing the Publisher and Year columns to take their default values.

ISBN             Title                 Author
---------------  --------------------  ---------------
978-6655443321   Big Data Analytics    Karen Adams

Note: The Publisher and Year columns will use their default values.
 
 
For example:

Test	Result
SELECT ISBN, Title, Author
FROM Books 


ISBN             Title                 Author
---------------  --------------------  ---------------
978-6655443321   Big Data Analytics    Karen Adams

```INSERT INTO Books(ISBN,Title,Author)
VALUES('978-6655443321','Big Data Analytics','Karen Adams');
```

**Output:**

<img width="1055" height="277" alt="image" src="https://github.com/user-attachments/assets/c6e0d762-af44-4fb9-b858-ec25f63071ca" />

**Question 6**
---
Create a new table named item with the following specifications and constraints:
item_id as TEXT and as primary key.
item_desc as TEXT.
rate as INTEGER.
icom_id as TEXT with a length of 4.
icom_id is a foreign key referencing com_id in the company table.
The foreign key should cascade updates and deletes.
item_desc and rate should not accept NULL.
For example:

Test	Result
INSERT INTO item VALUES("ITM5","Charlie Gold",700,"COM4");
UPDATE company SET com_id='COM5' WHERE com_id='COM4';
SELECT * FROM item;
item_id     item_desc     rate        icom_id
----------  ------------  ----------  ----------
ITM5        Charlie Gold  700         COM5

```CREATE TABLE item(
item_id TEXT primary key,
item_desc TEXT NOT NULL,
rate INTEGER NOT NULL,
icom_id TEXT(4),
foreign key(icom_id) REFERENCES company(com_id)
    ON UPDATE CASCADE
    ON DELETE CASCADE
);
```

**Output:**

<img width="1186" height="313" alt="image" src="https://github.com/user-attachments/assets/420a367e-e541-484c-ba69-826bdfb0c26f" />

**Question 7**
---
Write a SQL Query  to change the name of attribute "name" to "first_name"  and add mobilenumber as number ,DOB as Date in the table Companies. 

 

 

For example:

Test	Result
pragma table_info('Companies');
cid         name        type        notnull     dflt_value  pk
----------  ----------  ----------  ----------  ----------  ----------
0           id          int         0                       0
1           first_name  varchar(50  0                       0
2           address     text        0                       0
3           email       varchar(50  0                       0
4           phone       varchar(10  0                       0
5           mobilenumb  number      0                       0
6           DOB         Date        0                       0

```ALTER TABLE Companies RENAME COLUMN name TO first_name;
ALTER TABLE Companies ADD COLUMN mobilenumber number;
ALTER TABLE Companies ADD COLUMN DOB Date;
```

**Output:**

<img width="1278" height="362" alt="image" src="https://github.com/user-attachments/assets/366f7e69-b8e8-45e5-aa40-e39120cf0799" />

**Question 8**
---
Write a SQL query to Add a new column Mobilenumber as number in the Student_details table.

Sample table: Student_details

 cid              name             type             notnu  dflt_value  pk
---------------  ---------------  ---------------  -----  ----------  ----------
0                RollNo           int              0                  1
1                Name             VARCHAR(100)     1                  0
2                Gender           TEXT             1                  0
3                Subject          VARCHAR(30)      0                  0
4                MARKS            INT (3)          0                  0
For example:

Test	Result
pragma table_info('Student_details');
cid    name             type             notnu  dflt_value  pk
-----  ---------------  ---------------  -----  ----------  ----------
0      RollNo           int              0                  1
1      Name             VARCHAR(100)     1                  0
2      Gender           TEXT             1                  0
3      Subject          VARCHAR(30)      0                  0
4      MARKS            INT (3)          0                  0
5      Mobilenumber     number           0                  0
```
ALTER TABLE Student_details ADD COLUMN Mobilenumber number;
```

**Output:**

<img width="1272" height="338" alt="image" src="https://github.com/user-attachments/assets/f8b59d1e-0720-48ca-aa4e-116d0f6f901d" />

**Question 9**
---
Insert the following customers into the Customers table:

CustomerID  Name         Address     City        ZipCode
----------  -----------  ----------  ----------  ----------
302         Laura Croft  456 Elm St  Seattle     98101
303         Bruce Wayne  789 Oak St  Gotham      10001
For example:

Test	Result
SELECT * FROM Customers;

CustomerID  Name         Address     City        ZipCode
----------  -----------  ----------  ----------  ----------
302         Laura Croft  456 Elm St  Seattle     98101
303         Bruce Wayne  789 Oak St  Gotham      10001
```INSERT INTO Customers (CustomerID, Name ,Address,City,ZipCode)
values (302,'Laura Croft','456 Elm St','Seattle',98101);

INSERT INTO Customers (CustomerID, Name ,Address,City,ZipCode)
values (303,'Bruce Wayne','789 Oak St','Gotham',10001); 
```

**Output:**

<img width="1158" height="320" alt="image" src="https://github.com/user-attachments/assets/c3a5e22a-798b-4e41-afc5-d8ebb0e12002" />

**Question 10**
---
Insert all students from Archived_students table into the Student_details table.

cid         name        type        notnull     dflt_value  pk
----------  ----------  ----------  ----------  ----------  ----------
0           RollNo      INT           0                       1
1           Name        VARCHAR(100)  0                       0
2           Gender      VARCHAR(10)   0                       0
3           Subject     VARCHAR(50)   0                       0
4           MARKS       INT           0                       0
For example:

Test	Result
select * from student_details;
RollNo      Name           Gender      Subject     MARKS
----------  -------------  ----------  ----------  ----------
1           Alice Johnson  Female      Math        85
2           Bob Smith      Male        Science     90
3           Charlie Brown  Male        English     78

```INSERT INTO Student_details
SELECT * FROM Archived_students;
```

**Output:**

<img width="1155" height="265" alt="image" src="https://github.com/user-attachments/assets/acb087fe-0f47-40cc-92e7-507e2bd83f33" />


## RESULT
Thus, the SQL queries to implement different types of constraints and DDL commands have been executed successfully.

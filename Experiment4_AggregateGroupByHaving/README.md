# Experiment 4: Aggregate Functions, Group By and Having Clause

## AIM
To study and implement aggregate functions, GROUP BY, and HAVING clause with suitable examples.

## THEORY

### Aggregate Functions
These perform calculations on a set of values and return a single value.

- **MIN()** – Smallest value  
- **MAX()** – Largest value  
- **COUNT()** – Number of rows  
- **SUM()** – Total of values  
- **AVG()** – Average of values

**Syntax:**
```sql
SELECT AGG_FUNC(column_name) FROM table_name WHERE condition;
```
### GROUP BY
Groups records with the same values in specified columns.
**Syntax:**
```sql
SELECT column_name, AGG_FUNC(column_name)
FROM table_name
GROUP BY column_name;
```
### HAVING
Filters the grouped records based on aggregate conditions.
**Syntax:**
```sql
SELECT column_name, AGG_FUNC(column_name)
FROM table_name
GROUP BY column_name
HAVING condition;
```

**Question 1**
--
Write SQL query to extract the email domain from each patient's email address and count the number of patients with the same email domain.

Sample table: Patients Table
For example:

Result
EmailDomain  TotalPatients
-----------  -------------
example.com  10

```sql
select substr(Email, instr(Email, '@')+1) as EmailDomain, count(*)as TotalPatients from "Patients" group by EmailDomain;
```

**Output:**

<img width="653" height="402" alt="image" src="https://github.com/user-attachments/assets/23a6b6f6-1bcb-40e4-ac0f-2b9d0515f42a" />

**Question 2**
---
How many patients have insurance coverage valid in each year?

Sample table:Insurance Table

name               type
-----------------  ----------
InsuranceID        INTEGER
PatientID          INTEGER
InsuranceCompany   TEXT
PolicyNumber       TEXT
PolicyHolder       TEXT
ValidityPeriod     TEXT
For example:

Result
ValidityYear  TotalPatients
------------  -------------
2024          3
2025          1
2027          4
2031          2

```sql
select
    substr(ValidityPeriod, 1,4) AS ValidityYear,
    count(*) AS TotalPatients
from Insurance
group by ValidityYear;
```

**Output:**

<img width="693" height="446" alt="image" src="https://github.com/user-attachments/assets/bf75eee3-6ddc-4055-b75f-06813d26e502" />

**Question 3**
---
How many medical records were created in each month?

Sample table:MedicalRecords Table
For example:

Result
Month       TotalRecords
----------  ------------
2023-12     2
2024-01     6
2024-02     2

```sql
select 
    substr(Date,1,7) as Month,
    count(*) as TotalRecords
from MedicalRecords
group by Month;
```

**Output:**

<img width="616" height="496" alt="image" src="https://github.com/user-attachments/assets/8acc8a1c-6865-4982-8516-913632edce02" />

**Question 4**
---
Write a SQL query to find the number of employees whose age is greater than 32.
Sample table: employee
id
name
age
address
salary
1
Paul
32
California
20000
4
Mark
25
Richtown
65000
5
David
27
Texas
85000

For example:
Result
COUNT
----------
5

```sql
select count(*) as COUNT from employee where age>32;
```

**Output:**

<img width="360" height="376" alt="image" src="https://github.com/user-attachments/assets/b3219d49-f957-4c4a-96c3-2e404d5906a2" />

**Question 5**
---
Write a SQL query to calculate the total number of working hours of all employees

Sample table: employee1
For example:

Result
Total working hours
-------------------
111

```sql
select sum(workhour) as "Total working hours" from employee1;
```

**Output:**

<img width="536" height="395" alt="image" src="https://github.com/user-attachments/assets/71931ea2-3862-404d-9f1d-aea17f9f0d4e" />

**Question 6**
---
Write a SQL query to find the total amount of fruits with a unit type of 'LB'.

Note: Inventory attribute contains amount of fruits

Table: fruits

name        type
----------  ----------
id          INTEGER
name        TEXT
unit        TEXT
inventory   INTEGER
price       REAL
 

For example:

Result
total
----------
225

```sql
select sum(inventory) as total from fruits where unit='LB';
```

**Output:**

<img width="462" height="382" alt="image" src="https://github.com/user-attachments/assets/084d1749-6d24-40e4-ba09-dfdb7e8a4870" />

**Question 7**
---
Write a SQL query to find What is the age difference between the youngest and oldest employee in the company.

Table: employee

name        type
----------  ----------
id          INTEGER
name        TEXT
age         INTEGER
city        TEXT
income      INTEGER
For example:

Result
age_difference
--------------
13

```sql
select (max(age) - min(age)) as age_difference from employee;
```

**Output:**

<img width="437" height="390" alt="image" src="https://github.com/user-attachments/assets/8adc3dbb-3c76-40ee-8e68-b3dd06ef2b8a" />

**Question 8**
---
Write the SQL query that accomplishes the grouping of data by joining date (jdate), calculates the total work hours for each date, and excludes dates where the total work hour sum is not greater than 40.

Sample table: employee1
For example:

Result
jdate       SUM(workhour)
----------  -------------
2004.0      46
2006.0      46
```sql
select jdate, SUM(workhour) from employee1 group by jdate having SUM(workhour)>40;
```

**Output:**

<img width="610" height="466" alt="image" src="https://github.com/user-attachments/assets/1fcaca68-b3b0-443c-b7b4-72254b7aecc0" />

**Question 9**
---
Write the SQL query that accomplishes the grouping of data by age intervals using the expression (age/5)5, calculates the minimum age for each group, and excludes groups where the minimum age is not less than 25.

Sample table: customer1
For example:

Result
age_group   MIN(age)
----------  ----------
20          22

```sql
select (age/5)*5 as age_group, MIN(age) from customer1 group by age_group having MIN(age)<25;
```

**Output:**

<img width="558" height="384" alt="image" src="https://github.com/user-attachments/assets/c7a8fac1-95ae-4739-9ab2-58fea6141b43" />

**Question 10**
---
Write the SQL query that accomplishes the grouping of data by age, calculates the total income for each age group, and includes only those age groups where the total income sum is greater than 1,000,000.

Sample table: employee
For example:

Result
age         SUM(income)
----------  -----------
35          10000000
40          1350000
```sql
select age,SUM(income) from employee group by age having SUM(income)>1000000;
```

**Output:**

<img width="604" height="489" alt="image" src="https://github.com/user-attachments/assets/ed56c480-a154-49a4-a025-c957dfd2454a" />


## RESULT
Thus, the SQL queries to implement aggregate functions, GROUP BY, and HAVING clause have been executed successfully.

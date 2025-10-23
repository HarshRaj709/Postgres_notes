# PostgreSQL Commands Guide

## 📌 Enter PostgreSQL Shell
```sh
sudo -u postgres psql
```
```text
sudo: Allows you to run commands with elevated (superuser/root) privileges.

-u postgres: The -u flag means “run as user”; here, you're telling sudo to run the command as the postgres user (the default PostgreSQL superuser).

psql: This is the PostgreSQL interactive terminal used to execute SQL queries and manage databases.

Why use this?: PostgreSQL creates a separate Linux user called postgres. To access the PostgreSQL shell with full privileges, you often need to run psql as that user.

What you can remove:

If you're already logged in as the postgres user, you can just run psql.

If your current user has the right DB permissions, you can run psql -U your_db_user -d your_db_name.

```print(response.json())

## 📌 List All Databases
```sql
\l
```

# Create New DB / New User / New Role / New permissions grant
```sql
CREATE DATABASE scanner;
CREATE USER scanner WITH PASSWORD 'scanner';
GRANT ALL PRIVILEGES ON DATABASE scanner TO scanner;
```

## 📌 List All Users
```sql
\du
```

## 📌 Grant All Privileges to a User
```sql
GRANT ALL PRIVILEGES ON DATABASE "DB_NAME" TO username;
```

## 📌 Exit PostgreSQL Shell
```sql
\q
```

---

## 🚀 Outside PostgreSQL Environment

### ✅ Check PostgreSQL Connection
```sh
psql -U username -d database_name -h localhost
```

### ✅ Check PostgreSQL Service Status
```sh
sudo systemctl status postgresql
```

### ✅ Start PostgreSQL Service
```sh
sudo systemctl start postgresql
```

### ✅ Stop PostgreSQL Service
```sh
sudo systemctl stop postgresql
```

### ✅ Restart PostgreSQL Service
```sh
sudo systemctl restart postgresql
```

---

## 📌 Database Management

### ✅ Create a Database
```sql
CREATE DATABASE mydatabase;
```

### ✅ Delete a Database
```sql
DROP DATABASE mydatabase;
```

### ✅ Connect to a Database
```sql
\c mydatabase;
```

### ✅ Show Tables in a Database
```sql
\dt
```

### 📌 CRUD Operations

### ✅ Create a Table
```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100) UNIQUE
);
```

### ✅ See Schema of the Table
```
\d users
```

### ✅ Insert Single Data into a Table
```sql
INSERT INTO users (name, email) VALUES ('John Doe', 'john@example.com');
```

### ✅ Insert Multiple Data into a Table
```sql
INSERT INTO users (name, email) VALUES ('John Doe', 'john@example.com'),('John Doe', 'john@example.com);
```
### Note:- If you are providing all data in the query in sequence then you can remove fields name from query
```
INSERT INTO users VALUES ('John Doe', 'john@example.com'),('John Doe', 'john@example.com);
```

### ✅ View Table Data
```sql
SELECT * FROM users;
```
#### NOTE:- Here * represents columns like if you want to view only name then 
```sql
SELECT name FROM users;
```

### ✅ Update content of table
```sql
UPDATE users SET name='harsh' WHERE email = 'john@example.com';
```

### Update multiple rows at single time use CASE
```sql
update persons
SET age = CASE
WHEN NAME ='ashish' THEN 21
WHEN NAME ='kamal' THEN 25
WHEN NAME = 'sunny' THEN 20
WHEN NAME = 'harsh' THEN 25
END
WHERE NAME IN ('ashish','kamal','sunny','harsh');
```

### ✅ Delete Table Data
```sql
DELETE FROM users WHERE id = 1;
```

### ✅ Delete a Table
```sql
DROP TABLE users;
```

---

## 📌 Advance Database Management

#### DATATYPE - 

##### Numeric - INT, DOUBLE, DECIMAL, SMALLINT.....

#### Constraints - Rule applied on a column.
        PRIMARY KEY, NOT NULL , DEFAULT, SERIAL , UNIQUE

```sql
CREATE TABLE advance (
    ID INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    city VARCHAR(100) NOT NULL DEFAULT "LUCKNOW"
);
```

### Auto Increment

```sql
CREATE TABLE advance (
    ID SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    city VARCHAR(100) NOT NULL DEFAULT "LUCKNOW"
);
```
---

### Update the columns of the tables

```sql
ALTER TABLE users ALTER COLUMN fname VARCHAR(50) NOT NULL;
```

```SQL
    ALTER TABLE employees
    ALTER COLUMN fullname TYPE INTEGER USING fullname::integer,
    ALTER COLUMN fullname SET NOT NULL,
    ALTER COLUMN fullname SET DEFAULT 20;
```

```sql
    ALTER TABLE employees
    ALTER COLUMN fullname
    SET DATA TYPE VARCHAR(200);
```

### ADD COLUMN in the table

```sql
ALTER TABLE users ADD COLUMN age INT NOT NULL DEFAULT 18;
```

### Update Column Name
```sql
ALTER TABLE users
RENAME COLUMN id TO User_id;
```

# 📌 TABLE RELATED PSQL commands

## Change Table name
```sql
ALTER TABLE employee
RENAME TO employees;
```

#### if want to change 2 things at same time then use ALTER 2 times
```sql
ALTER TABLE employee 
ALTER COLUMN salary TYPE DECIMAL(10,2),
ALTER COLUMN salary SET DEFAULT 30000;
```

#### Need to use SET when changing Any constraints and TYPE when changing any DATA TYPE.
```SQL
ALTER TABLE employee
bankdb-# ALTER COLUMN hire_date SET NOT NULL,
bankdb-# ALTER COLUMN hire_date SET DEFAULT CURRENT_DATE;
```
---

### CLAUSES - 
#### Order_by, Where, Distinct, Limit, Like

1. Where
```sql
SELECT fname FROM employee
Where dept = 'IT';
```

```sql
    SELECT * FROM employee
    WHERE salary > 100000;
```

```sql
    SELECT * FROM employee
    WHERE dept='IT' or dept='HR';
```

```sql
    SELECT * FROM employee
    WHERE dept = 'IT' and salary < 100000;
```

```sql
    SELECT * FROM employee
    WHERE dept!='IT';
```

## TO Reduce redundancy of OR we can use IN

```sql
    SELECT * FROM employee
    WHERE dept ='IT' 
    OR dept = 'HR'
    OR dept = 'FIN';
```

```sql
    SELECT * FROM employee
    WHERE dept IN ('IT','HR','FIN');
```

```sql
    SELECT * FROM employee
    WHERE dept NOT IN ('HR','MKT');
```

2. RANGE

```sql
SELECT * FROM employee
WHERE salary BETWEEN 10000 and 350000;
```

3. DISTINCT - To get unique value of a column

```sql
SELECT DISTINCT dept FROM employee;
```

4. Order by -- Sorting -- by default ASC;

```sql
SELECT * FROM employee
ORDER BY fname;
```

4.  DESCENDING ORDER

```sql
    SELECT * FROM employee
    ORDER BY emp_id DESC;
```

5. LIMIT

```sql
SELECT * FROM employee LIMIT 3;
```

6. LIKE -- USED TO FIND PATTERN like to get all name starting with 'A'

```sql
SELECT * FROM employee
WHERE fname LIKE 'A%';          --- CASE SENSITIVE
```

```sql
    SELECT * FROM employee
    WHERE fname LIKE '%A';      --- at last
```

```sql
    SELECT * FROM employee
    WHERE fname LIKE '%A%';     --- in between
```

```sql
    SELECT * FROM employee
    WHERE dept LIKE '__';      --- 2 length dept  'HR', 'IT'
```

```sql
    SELECT * FROM employee
    WHERE fname LIKE '_a%';      --- 2nd place have a
```


### AGGREGATE FUNCTION

#### COUNT, SUM, AVERAGE, MIN, MAX

1. COUNT

```sql
    SELECT COUNT(fname) FROM employee;
```

2. SUM

```sql
    SELECT SUM(salary) FROM employee;           --- 1680000.00
```

3. AVERAGE

```sql
    SELECT AVG(salary) FROM employee;           --- 210000.000000000000
```

4. MIN

```sql
    SELECT MIN(salary) FROM employee;           --- 50000.00
```

5. MAX

```sql
    SELECT MAX(salary) FROM employee;           --- 500000.00
```

## GROUP BY     -- COMBINE ENTITIES WITH SOME SIMILIARITY (“I want one row per group, and all the other selected columns must make sense within that group.”)

```sql
SELECT dept, COUNT(fname) AS total FROM employee --- TOTAL EMPLOYEE INDEPARTMENT          
GROUP BY dept;                                               dept | total 
                                                            ------+-------
                                                             FIN  |     1
                                                             OPS  |     1
                                                             MKT  |     1
                                                             IT   |     3
                                                             HR   |     2
```

```sql
    SELECT dept, ARRAY_AGG(EMP_ID) AS emp_ids
    FROM employee                                   --- EMP_ID OF ALL DEPARTMENT
    GROUP BY dept;
                                                             dept | emp_ids 
                                                            ------+---------
                                                            FIN  | {6}
                                                            OPS  | {8}
                                                            MKT  | {7}
                                                            IT   | {2,3,5}
                                                            HR   | {1,4}
```

```sql
    SELECT dept, SUM(salary)
    FROM employee                                   --- Total salary of DEPARTMENT
    GROUP BY dept;
                                                             dept |    sum    
                                                            ------+-----------
                                                            FIN  |  62000.00
                                                            OPS  |  89000.00
                                                            MKT  |  54000.00
                                                            IT   | 925000.00
                                                            HR   | 550000.00
```

```sql
    SELECT dept, SUM(salary)
    FROM employee
    WHERE dept = 'IT'                               --- TOTAL SALARY OF IT DEPARTMENT
    GROUP BY dept;
                                                             dept |  salary   
                                                            ------+-----------
                                                            IT   | 925000.00
```

## STRING FUNCTIONS

#### CONCAT, CONCAT_WS, SUBSTR, LEFT, RIGHT, LENGTH, UPPER, LOWER, TRIM, LTRIM, RTRIM,
#### REPLACE, POSITION, STRING_AGG

1. CONCAT       --- CONCAT(first_col, sec_col),    CONCAT(first_word, sec_word...)

```sql
SELECT CONCAT(fname,lname) AS FULLNAME FROM employee;

                    or

SELECT fname || ' ' || lname AS full_name FROM employee;

                                                                        fullname    
                                                                    ----------------
                                                                    ashishbajpai
                                                                    rahulsingh
                                                                    harshrajpt
                                                                    RahulSharma
                                                                    PriyaVerma
                                                                    AmitSingh
                                                                    SnehaKapoor
                                                                    VikramMalhotra
```

#### We can use || to cancat strings

```sql
    SELECT 'Post' || 'greSQL' AS result;
```

2. CONCAT_WS  --- CONCAT WITH SEPERATOR  -- CONCAT_WS('-',fname,lname)

```sql
SELECT CONCAT_WS(' ',fname,lname) AS FULLNAME FROM employee;
```

3. SUBSTRING   -- SELECT SUBSTRING('HEY BUDDY,1,4); OR SELECT SUBSTR('HEY BUDDY,1,4);

```sql
SELECT SUBSTR('HEY BUDDY',1,4);
```

4. REPLACE   --- REPLACE(STR,FROM_STR,TO_STR)

```sql
SELECT REPLACE('Hey BUDDY','Hey','HELLO');
SELECT REPLACE(dept,'IT','TECH') FROM employee;
                                                            replace 
                                                            ---------
                                                            TECH
                                                            TECH
                                                            HR
                                                            HR
                                                            TECH
                                                            FIN
                                                            MKT
                                                            OPS 
```

```sql
SELECT emp_id, fname, lname, REPLACE(dept, 'IT', 'TECH') AS dept, salary
FROM employee;
                                            emp_id | fname  |  lname   | dept |  salary   
                                            --------+--------+----------+------+-----------
                                                2 | ashish | bajpai   | TECH | 350000.00
                                                3 | rahul  | singh    | TECH | 500000.00
                                                1 | harsh  | rajpt    | HR   | 500000.00
                                                4 | Rahul  | Sharma   | HR   |  50000.00
                                                5 | Priya  | Verma    | TECH |  75000.00
                                                6 | Amit   | Singh    | FIN  |  62000.00
                                                7 | Sneha  | Kapoor   | MKT  |  54000.00
                                                8 | Vikram | Malhotra | OPS  |  89000.00
```

5. REVERSE

```sql
SELECT REVERSE(fname) FROM employee;
```

6. LENGTH

```sql
SELECT LENGTH(fname) FROM employee;             ---
                                                    length 
                                                    --------
                                                        6
                                                        5
                                                        5
                                                        5
                                                        5
                                                        4
                                                        5
                                                        6
```

##### Return all the details of employee whose fname have length less than 5

```sql
    SELECT emp_id,fname,lname,salary,dept FROM employee
    WHERE LENGTH(fname)<5;
```

7. UPPER and LOWER

```sql
SELECT UPPER(*) FROM employee;
```

```sql
SELECT LOWER(*) FROM employee;
```

#### Want to chnage multiple columns then use 

```sql
    SELECT UPPER(fname || ' ' || lname || ' - ' || dept) AS details FROM employee;
```

8. LEFT and RIGHT -- Easy substring

```sql
SELECT LEFT('HELLO WORLD',5);   ----  HELLO
SELECT RIGHT('HELLO WORLD',5);   ----  WORLD
```

9. TRIM         --- TO Remove Extra Space from starting and ending only

```sql
SELECT TRIM('  HARSH SAHU     !');   ----  HARSH SAHU     !
```

10. POSITION   --- to find SUBSTR

```sql
SELECT POSITION('om' IN 'thomas');
```

## Query Inside a Query --- can return all columns which passed the filter
```sql
SELECT * FROM employee
WHERE salary = (SELECT MAX(salary) FROM employee);

emp_id | fname | lname |         email          | dept |  salary   | hire_date  
--------+-------+-------+------------------------+------+-----------+------------
      3 | rahul | singh | myemail@gmail.com      | IT   | 500000.00 | 2025-04-02
      1 | harsh | rajpt | harshsahu709@gmail.com | HR   | 500000.00 | 2022-02-21
```


# 📌 ADD CHECK CONSTRAINTS- TO verify something

## Updating column

```sql
ALTER TABLE users
ADD CONSTRAINT min_name_len CHECK (LENGTH(name)>4);
```

## Creating new table and Add CHECK

```sql
    CREATE TABLE area(
        name VARCHAR(100),
        mob VARCHAR(15) UNIQUE CHECK (LENGTH(mob) >=10)
    );
```

# 📌 Named Constraints

```sql
    CREATE TABLE checks (
    name VARCHAR(10),
    CONSTRAINT min_len_name CHECK (LENGTH(name) > 4)
);

```

## DROP Constraint CHECk
```sql
ALTER TABLE area
DROP CONSTRAINT min_name_len;
```

# 📌 ADDITIONAL TOPICS

## 1. CASE

```sql
    SELECT fullname,salary,
    CASE
    WHEN salary >= 350000 THEN 'HIGHER SALARY'
    ELSE 'LOWER SALARY'
    END AS salary_category
    FROM employees;                                      fullname |  salary   | salary_category 
                                                    ----------+-----------+-----------------
                                                    ashish   | 350000.00 | HIGHER SALARY
                                                    rahul    | 500000.00 | HIGHER SALARY
                                                    harsh    | 500000.00 | HIGHER SALARY
                                                    Rahul    |  50000.00 | LOWER SALARY
                                                    Priya    |  75000.00 | LOWER SALARY
                                                    Amit     |  62000.00 | LOWER SALARY
                                                    Vikram   |  89000.00 | LOWER SALARY
                                                    Sneha    |  54000.00 | LOWER SALARY
```

### Multiple Conditions
```sql
    SELECT *,
    CASE
    WHEN salary >= 350000 THEN 'HIGH'
    WHEN salary > 62000 AND salary < 350000 THEN 'MID'
    ELSE 'LOW'
    END AS salary_categor
    FROM employees;
```

### ONLY SHOW TOTAL EMPLOYEES ACCORDING TO THEIR SALARY CATEGORY
```sql
    SELECT 
        CASE
            WHEN salary >= 350000 THEN 'HIGH'
            WHEN salary > 62000 AND salary < 350000 THEN 'MID'
            ELSE 'LOW'
        END AS salary_category,
        COUNT(*) AS total_employees
    FROM employees
    GROUP BY 
        salary_category;                                    salary_category | total_employees 
                                                            -----------------+-----------------
                                                            MID             |               2
                                                            HIGH            |               3
                                                            LOW             |               3
```

### NEED TO CALCULATE BONUS WHICH IS 10% OF THEIR SALARY
```sql
    SELECT fullname,salary,ROUND(salary / 10,2) AS bonus
    FROM employees;                                     fullname |  salary   |  bonus   
                                                        ----------+-----------+----------
                                                        ashish   | 350000.00 | 35000.00
                                                        rahul    | 500000.00 | 50000.00
                                                        harsh    | 500000.00 | 50000.00
                                                        Rahul    |  50000.00 |  5000.00
                                                        Priya    |  75000.00 |  7500.00
                                                        Amit     |  62000.00 |  6200.00
                                                        Vikram   |  89000.00 |  8900.00
                                                        Sneha    |  54000.00 |  5400.00
```

### Update multiple rows at single time use CASE
```sql
    update persons
    SET age = CASE
    WHEN NAME ='ashish' THEN 21
    WHEN NAME ='kamal' THEN 25
    WHEN NAME = 'sunny' THEN 20
    WHEN NAME = 'harsh' THEN 25
    END
    WHERE NAME IN ('ashish','kamal','sunny','harsh');
```


# 📌 RELATIONSHIPS

## TYPES OF RELATIONSHIPS
    1. ONE-TO-ONE
    2. MANY-TO-ONE
    3. MANY-TO-MANY

### Link ONE-TO-MANY PSQL
 <a href="foreign.md">link</a> 

### LINK MANY-TO-MANY
<a href="many.md">link</a>

---------------------------------------------------------------------------------------

# 📌 Views
What are Views in PostgreSQL?
<br>
A view in PostgreSQL is a virtual table based on the result of a SQL query. It behaves like a regular table in queries, but it doesn’t store data itself — it pulls fresh data each time it's queried.

## Access all views of the db
```sql
\dv
```


```sql
CREATE VIEW billing AS
SELECT 
	c.cust_name,
	o.ord_date,
	p.p_name,
	p.price,
	oi.quantity,
	(oi.quantity*p.price) AS total_price
FROM order_items oi
	JOIN
		products p ON oi.p_id=p.p_id
	JOIN
		orders o ON o.ord_id=oi.ord_id
	JOIN
		customers c ON o.cust_id=c.cust_id;
```

## Now to view that we dont need to write the whole query again

```sql
    SELECT * FROM billing;
```

## Grouping

```sql
SELECT p_name, SUM(total_price) 
FROM billing
GROUP BY p_name;                                  p_name  |    sum    
                                                ----------+-----------
                                                Cable    |   1750.00
                                                Mouse    |       500
                                                Keyboard |    800.00
                                                Laptop   | 110000.00
```

# 📌 HAVING CLAUSE

## Filtering using WHERE will not work with GROUP BY  use HAVING to apply condition

```sql
SELECT p_name, SUM(total_price)
FROM billing
GROUP BY p_name
WHERE SUM(total_price) > 800;               ERROR:  syntax error at or near "WHERE"
                                            LINE 4: WHERE SUM(total_price) > 800;
```

### Correct Query

```sql
SELECT p_name, SUM(total_price)
FROM billing
GROUP BY p_name
HAVING SUM(total_price) > 800;                       p_name |    sum    
                                                    --------+-----------
                                                    Cable  |   1750.00
                                                    Laptop | 110000.00
```

---------------------------------------------------------------------------------------

# 📌 GROUP BY ROLLUP : To add total of column

## COALESCE is a SQL function that returns the first non-null value from a list of expressions.
```sql
SELECT COALESCE(NULL, NULL, 'Hello', 'World');
-- Output: 'Hello'
```

```sql
SELECT 
    COALESCE(p_name, 'TOTAL'),
    SUM(total_price) AS amount
FROM billing
GROUP BY 
    ROLLUP(p_name)
    ORDER BY amount;                         coalesce |  amount   
                                            ----------+-----------
                                            Mouse    |       500
                                            Keyboard |    800.00
                                            Cable    |   1750.00
                                            Laptop   | 110000.00
                                            TOTAL    | 113050.00
```

# 📌 STORED ROUTINE : These are sets of SQL statements that are saved in the database and can be executed repeatedly. They're useful for encapsulating complex logic, reusing code, improving performance, and enforcing business rules
<br>

# 1. Stored Procedure: Set of SQL statements and procedural logic that can perform operations such as inserting, updating, deleting, and querying data.

## SYNTAX
```sql
CREATE OR REPLACE PROCEDURE procedure_name (parameter_name,parameter_type,..)
LANGUAGE plpgsql
AS $$
BEGIN 
    ---     PROCEDURAL CODE
END;
$$;
```

### EXAMPLE :- Repetative query, to solve this problem we use STORED ROUTINE.

```sql
UPDATE employees
SET salary = 97000
WHERE emp_id = 4;
```

### STORED PROCEDURE FOR THAT

```sql
CREATE OR REPLACE PROCEDURE update_price(
p_id INT,
price NUMERIC
)
LANGUAGE plpgsql
AS $$
BEGIN
UPDATE products
SET price=price
WHERE p_id = p_id;
END;
$$;
)                                               CALL update_price(2,300);
                                                ERROR:  column reference "p_id" is ambiguous
                                                LINE 3: WHERE p_id = p_id
                                                            ^
                                                DETAIL:  It could refer to either a PL/pgSQL variable or a table column.
                                                QUERY:  UPDATE products
                                                SET price=price
                                                WHERE p_id = p_id
                                                CONTEXT:  PL/pgSQL function update_price(integer,numeric) line 3 at SQL statement
```

## To solve that error use different names for parameters

```sql
CREATE OR REPLACE PROCEDURE update_price(
    in_p_id INT,
    in_price NUMERIC
)
LANGUAGE plpgsql
AS $$
BEGIN
    UPDATE products
    SET price = in_price
    WHERE p_id = in_p_id;
END;
$$;
```

## Use it using call
```sql
CALL update_price(4,300);
```

## To DROP PROCEDURE
```sql
DROP PROCEDURE update_price(integer,numeric);
```

## List all procedures
```sql
SELECT proname 
FROM pg_proc 
WHERE prokind = 'p';
```

# 2. USER DEFINED FUNCTIONS: Custom functions created by the user to perform specific operations and return a value.

# Main difference between this and stored procedure is that it return something while procedure not.

## SYNTAX

```sql
CREATE OR REPLACE FUNCTION function_name (parameters)
RETURN return_type AS $$
BEGIN
    --FUNCTION BODY (SQL STATEMENT)
    RETURN some_value;
END;
$$ LANGUAGE plpgsql;
```

## Find name of employee in each department having maximum salary.

## What i tried

```sql
SELECT dept, MAX(salary)
FROM employees
GROUP BY dept;                                                   dept |    max    
                                                                ------+-----------
                                                                FIN  |  62000.00
                                                                OPS  |  89000.00
                                                                MKT  |  54000.00
                                                                IT   | 500000.00
                                                                HR   | 500000.00
```
## Problem with this cannot write this multiple times so use user defined function
```sql
SELECT 
    emp_id,
    fname,
    salary
FROM employees
WHERE dept = 'HR'
AND salary = (
    SELECT MAX(salary)
    FROM employees
    WHERE dept = 'HR'
);                                                   emp_id | fname |  salary   
                                                    --------+-------+-----------
                                                        1 | harsh | 500000.00

```
## List all functions
```sql
\df
```

## User defined function for that

```sql
CREATE OR REPLACE FUNCTION max_sal_emp1(dept_name VARCHAR)
RETURNS TABLE(emp_id INT, fname VARCHAR, salary NUMERIC)
AS $$
BEGIN
    RETURN QUERY
    SELECT
        e.emp_id, e.fname, e.salary
    FROM 
        employees e
    WHERE
        e.dept = dept_name
        AND e.salary = (
            SELECT MAX(emp.salary)
            FROM employees emp
            WHERE emp.dept = dept_name
        );
END;
$$
LANGUAGE plpgsql;                                    emp_id | fname |  salary   
                                                    --------+-------+-----------
                                                        3 | rahul | 500000.00
```

## CALL FUNCTION
```sql
SELECT * FROM max_sal_emp1('IT');
```

## DROP FUNCTION

```sql
DROP FUNCTION max_sal_emp1;
```

# WINDOWS FUNCTIONS : It allows to perform calculations across a set of rows related to the current row. --- DEFINED BY AN OVER() clause.

```sql
SELECT fname,
    SUM(salary)
FROM employees;

ERROR:  column "employees.fname" must appear in the GROUP BY clause or be used in an aggregate function
LINE 1: SELECT fname,
```

```sql
SELECT fname,salary,
    SUM(salary) OVER()
FROM employees;                                             fname  |  salary   |    sum     
                                                            --------+-----------+------------
                                                            ashish | 350000.00 | 1680000.00
                                                            rahul  | 500000.00 | 1680000.00
                                                            harsh  | 500000.00 | 1680000.00
                                                            Rahul  |  50000.00 | 1680000.00
                                                            Priya  |  75000.00 | 1680000.00
                                                            Amit   |  62000.00 | 1680000.00
                                                            Vikram |  89000.00 | 1680000.00
                                                            Sneha  |  54000.00 | 1680000.00
```
## Running Calculation

```sql
SELECT fname, salary, SUM(salary)
    OVER(ORDER BY salary)
    FROM employees;                                      fname  |  salary   |    sum     
                                                        --------+-----------+------------
                                                        Rahul  |  50000.00 |   50000.00
                                                        Sneha  |  54000.00 |  104000.00
                                                        Amit   |  62000.00 |  166000.00
                                                        Priya  |  75000.00 |  241000.00
                                                        Vikram |  89000.00 |  330000.00
                                                        ashish | 350000.00 |  680000.00
                                                        harsh  | 500000.00 | 1680000.00
                                                        rahul  | 500000.00 | 1680000.00
```

### ROW NUMBER()
```sql
SELECT ROW_NUMBER() OVER(),
    fname,dept,salary, SUM(salary)
    OVER(ORDER BY salary)
    FROM employees;                              row_number | fname  | dept |  salary   |    sum     
                                                ------------+--------+------+-----------+------------
                                                        1 | Rahul  | HR   |  50000.00 |   50000.00
                                                        2 | Sneha  | MKT  |  54000.00 |  104000.00
                                                        3 | Amit   | FIN  |  62000.00 |  166000.00
                                                        4 | Priya  | IT   |  75000.00 |  241000.00
                                                        5 | Vikram | OPS  |  89000.00 |  330000.00
                                                        6 | ashish | IT   | 350000.00 |  680000.00
                                                        7 | harsh  | HR   | 500000.00 | 1680000.00
                                                        8 | rahul  | IT   | 500000.00 | 1680000.00
```

```sql
SELECT ROW_NUMBER() OVER(PARTITION BY dept),
    fname,dept,salary, SUM(salary)
    OVER(ORDER BY salary)
    FROM employees;                              row_number | fname  | dept |  salary   |    sum     
                                                ------------+--------+------+-----------+------------
                                                        1 | Amit   | FIN  |  62000.00 |  166000.00
                                                        1 | Rahul  | HR   |  50000.00 |   50000.00
                                                        2 | harsh  | HR   | 500000.00 | 1680000.00
                                                        1 | rahul  | IT   | 500000.00 | 1680000.00
                                                        2 | Priya  | IT   |  75000.00 |  241000.00
                                                        3 | ashish | IT   | 350000.00 |  680000.00
                                                        1 | Sneha  | MKT  |  54000.00 |  104000.00
                                                        1 | Vikram | OPS  |  89000.00 |  330000.00
```

```sql
SELECT
    fname,dept,salary,
    RANK() OVER(ORDER BY salary DESC)
    FROM employees;                                      fname  | dept |  salary   | rank 
                                                        --------+------+-----------+------
                                                        rahul  | IT   | 500000.00 |    1
                                                        harsh  | HR   | 500000.00 |    1
                                                        ashish | IT   | 350000.00 |    3
                                                        Vikram | OPS  |  89000.00 |    4
                                                        Priya  | IT   |  75000.00 |    5
                                                        Amit   | FIN  |  62000.00 |    6
                                                        Sneha  | MKT  |  54000.00 |    7
                                                        Rahul  | HR   |  50000.00 |    8
```

# TRIGGERS: Triggers are special procedures in a database that automatically execute predefined actions in resonse to certain events on a specified table or view.

## SYNTAX
```sql
CREATE TRIGGER trigger_name
{BEFORE | AFTER | INSTEAD OF } { INSERT | UPDATE | DELETE | TRUNCATE }
ON table_name
FOR EACH { ROW | STATEMENT }
EXECUTIVE FUNCTION trigger_fuction_name();
```

##
```sql
CREATE OR REPLACE FUNCTION trigger_function_name()
RETURNS TRIGGER AS $$
BEGIN
    -- Trigger logic here
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;
```

## Use Case
```text
Create a trigger so that if we insert/update negative salary in a table, it will be triggered amd set it to 0.
```

## Example
```sql
CREATE OR REPLACE FUNCTION check_salary()
    RETURNS TRIGGER AS $$
    BEGIN
        IF NEW.salary < 0 THEN
            NEW.salary = 0;
        END IF;
        RETURN NEW;
    END;
    $$ LANGUAGE plpgsql;
```

## CREATE TRIGGER before_update_salary
```sql
CREATE TRIGGER before_update_salary
BEFORE UPDATE ON employees
FOR EACH ROW
EXECUTE FUNCTION check_salary();
```


# 📌 User Management

### ✅ Create a New User
```sql
CREATE USER new_user WITH PASSWORD 'password';
```

### ✅ Grant Privileges to a User
```sql
GRANT ALL PRIVILEGES ON DATABASE mydatabase TO new_user;
```

### ✅ Change User Password
```sql
ALTER USER new_user WITH PASSWORD 'new_password';
```

### ✅ Delete a User
```sql
DROP USER new_user;
```

---

## 📌 Backup & Restore

### ✅ Backup a Database
```sh
pg_dump -U username -d database_name -f backup.sql
```

### ✅ Restore a Database
```sh
psql -U username -d database_name -f backup.sql
```

---

## 📌 Logs & Debugging

### ✅ View PostgreSQL Logs
```sh
sudo journalctl -u postgresql --no-pager | tail -n 50
```

### ✅ Check Active Connections
```sql
SELECT * FROM pg_stat_activity;
```

---

This guide covers essential PostgreSQL commands for managing databases, users, and debugging common issues. 🚀


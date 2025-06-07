# Many-TO-Many

## Requires 3 tables to connect 2 tables, one will hold primary keys of other two table
```sql
 s_id |  name   
------+---------
    1 | HARSH
    2 | ashish
    3 | amitabh
```

```sql
 c_id |  name  |  fee  
------+--------+-------
    1 | python |  1500
    2 | c++    |  2500
    3 | java   | 12000
```

```sql
 enroll_no | s_id | c_id | enroll_date 
-----------+------+------+-------------
         6 |    1 |    1 | 2024-01-29
         7 |    1 |    2 | 2024-01-19
         8 |    2 |    1 | 2024-02-01
         9 |    2 |    3 | 2024-02-13
        10 |    3 |    3 | 2024-03-25
```

# Now i want to show student's name and course name instead of their ids in table

```sql
SELECT s.name,c.name,c.fee
FROM enrollment e
JOIN students s ON s.s_id = e.s_id
JOIN courses c ON c.c_id = e.c_id
;                                                     name   |  name  |  fee  
                                                    ---------+--------+-------
                                                    HARSH   | python |  1500
                                                    HARSH   | c++    |  2500
                                                    ashish  | python |  1500
                                                    ashish  | java   | 12000
                                                    amitabh | java   | 12000
```

## Combines all tables
```sql
SELECT *                  
FROM enrollment e
JOIN students s ON s.s_id = e.s_id
JOIN courses c ON c.c_id = e.c_id
;
    enroll_no | s_id | c_id | enroll_date | s_id |  name   | c_id |  name  |  fee  
    -----------+------+------+-------------+------+---------+------+--------+-------
            6 |    1 |    1 | 2024-01-29  |    1 | HARSH   |    1 | python |  1500
            7 |    1 |    2 | 2024-01-19  |    1 | HARSH   |    2 | c++    |  2500
            8 |    2 |    1 | 2024-02-01  |    2 | ashish  |    1 | python |  1500
            9 |    2 |    3 | 2024-02-13  |    2 | ashish  |    3 | java   | 12000
           10 |    3 |    3 | 2024-03-25  |    3 | amitabh |    3 | java   | 12000
```


---------------------------------------------------------------------------------------

# Task
```text
Create a one-to-many and many-to-many relationship in a shopping store context using four tables.

Customers
orders
products
order_items

Include a price column in the products table and display the relationship between customers and their orders along with the details of the products in each order.
```

# To view the complete data

```sql
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

 cust_name |  ord_date  |  p_name  |  price   | quantity | total_price 
-----------+------------+----------+----------+----------+-------------
 HARSH     | 2024-01-01 | Laptop   | 55000.00 |        1 |    55000.00
 HARSH     | 2024-01-01 | Cable    |   250.00 |        2 |      500.00
 ANSHUMAN  | 2024-02-01 | Laptop   | 55000.00 |        1 |    55000.00
 veer      | 2024-03-01 | Mouse    |      500 |        1 |         500
 veer      | 2024-03-01 | Cable    |   250.00 |        5 |     1250.00
 ANSHUMAN  | 2024-04-04 | Keyboard |   800.00 |        1 |      800.00
```


<a href="Postgres_notes.md">back</a>
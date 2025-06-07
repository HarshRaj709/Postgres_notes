# Original Table
```sql
CREATE TABLE customers(
cust_id SERIAL PRIMARY KEY,
cust_name VARCHAR(50) NOT NULL
);
```


# FOreign Table
```sql
CREATE TABLE ORDERS(
ord_id SERIAL PRIMARY KEY,
ord_date DATE NOT NULL DEFAULT CURRENT_DATE,
price NUMERIC NOT NULL,
cust_id INTEGER NOT NULL,
FOREIGN KEY(cust_id) REFERENCES customers(cust_id) ON DELETE CASCADE
);
```


# HOW TO VIEW RELATED FIELDS        (JOIN)

## INNER JOIN -  Returns only those rows where there is a match between the specified columns.

```sql
SELECT * 
FROM customers
JOIN
orders ON customers.cust_id = orders.cust_id;
```


```sql
SELECT customers.cust_id,
customers.cust_name,
orders.ord_date AS order_date,
orders.price AS total
FROM customers
JOIN
orders ON orders.cust_id = customers.cust_id;

         cust_id | cust_name | order_date | total  
        ---------+-----------+------------+--------
            1 | harsh     | 2025-11-15 | 250.00
            1 | harsh     | 2025-10-31 | 255.00
            2 | anirudh   | 2025-10-13 |    350
            3 | bakasur   | 2025-01-05 |    432
```
## only those are showing which have orders

##

# LEFT JOIN - to see all users even who dont have any orders
```sql
SELECT 
    customers.cust_id AS cust_id,
    customers.cust_name AS customer_name,
    orders.ord_id AS order_id,
    orders.ord_date AS order_date
FROM 
    customers
LEFT JOIN 
    orders ON customers.cust_id = orders.cust_id;      cust_id | customer_name | order_id | order_date 
                                                    ---------+---------------+----------+------------
                                                        1 | harsh         |        1 | 2025-11-15
                                                        1 | harsh         |        2 | 2025-10-31
                                                        2 | anirudh       |        3 | 2025-10-13
                                                        3 | bakasur       |        4 | 2025-01-05
                                                        4 | shiv          |          | 
```

# To see all orphand orders
```sql
SELECT 
    orders.*
FROM 
    orders
LEFT JOIN users ON orders.user_id = users.id
WHERE users.id IS NULL;
```

# Grouping of Customers according to there name

```sql
SELECT customers.cust_name, COUNT(orders.ord_id)
FROM customers
JOIN
orders
ON orders.cust_id = customers.cust_id
GROUP BY customers.cust_name;                                       cust_name | count 
                                                                    -----------+-------
                                                                    harsh     |     2
                                                                    anirudh   |     1
                                                                    bakasur   |     1
```



<a href="Postgres_notes.md">back</a>
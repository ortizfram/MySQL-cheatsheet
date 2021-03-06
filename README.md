
# MySQL_cheatsheet my sql commands
Here the scripts that where used for examples ð (most of them from first folder)

https://bit.ly/3rvtqdO

###### ðcommented code is like this â¤µï¸
` --` SELECT * FROM customers

## 1 Use dataBase
database
```
USE database_name;
```
-------------------------------------------
# # ð¢ SELECT

all or
column
```
SELECT *
SELECT customer_id, fisrt_name
```
- FUNTIONS



ð¤ EXAMPLE

ð§® calculate discount w user_points

 `AS var_name`
```
SELECT first_name, points, (points * 10) + 100 AS 'discount factor'
FROM customers
```
aritmatic expression (+,-,/,*,% (reminder of the division))

---------------------------------------
# # ð¢  FROM, WHERE, AND, OR, NOT

- `FROM`

table  (clause words)
```
FROM customers 
```

- `WHERE`
â¹ï¸ (date values are always represented between  ' ' )
```
WHERE customer_id = 1
WHERE points > 300
WHERE state = 'VA'
WHERE birth_date > '1990-01-01'
```

ð¤ EXAMPLE

```
SELECT *
FROM orders
WHERE order_date >= '2019-01-01' 
```


- MULTIPLE CONDITIONS `AND`, `OR`


ð¤ EXAMPLE

ð§® birth date gratter than 2019, AND +1000 points

ð§® OR = 1 of 2 conditions AND state = 'virginia'
```
SELECT *
FROM customers
WHERE birth_date >= '1990-01-01' AND points > 1000

SELECT *
FROM customers
WHERE birth_date >= '2019-01-01' 
(OR points > 1000 AND state = 'VA')
```
- `NOT`
```
SELECT *
FROM customers
WHERE NOT (birth_date >= '1990-01-01' AND points > 1000)
```
ð¤ EXAMPLE

ð§® get items FOR ORDER #6, WHERE total price greater than 30

```
SELECT * 
from order_items
WHERE order_id = 6 AND unit_price * quantity > 30
```

-------------------------------------
# # ð¢ OPERATORS // NOT, IN, BETWEEN, LIKE, REGEXP, IS NULL

- `IN`, `NOT IN` operator

â¹ï¸ same as OR but you can write all values together like this :

ð¤ EXAMPLE
```
SELECT *
FROM customers
WHERE  state IN ('VA', 'FL', 'GA')
```


- `BETWEEN`

â¹ï¸ same as <=, >=

ð¤ EXAMPLE

```
SELECT *
FROM customers
WHERE points BETWEEN 1000 AND 3000


SELECT *
FROM customers
WHERE birth_date BETWEEN '1-1-1990' AND '2000-1-1'
```

- `LIKE` 

ð¤ EXAMPLE

â¹ï¸ you put the **%** to declare what you re looking for 

ð¥ LIKE 'b%' = STARTs w B

ð¥ LIKE '%b%' = has a B ANYWhERE

ð¥ LIKE '%b' = ENDs w B


ð¥ LIKE '_y' = SECOND CHAR equals Y

ð§® when string starts like **b%**:... 

```
SELECT *
FROM customers
WHERE last_name LIKE 'b%'

SELECT *
FROM customers
WHERE last_name LIKE '%b%'

SELECT *
FROM customers
WHERE last_name LIKE '%b'


```
- `REGEXP`

â¹ï¸ same as LIKE 

ð¥ REGEXP '^field' = phrase STARTs w field...

ð¤EXAMPLE ð§® starts w : my , or contains: SE
```
SELECT *
FROM customers
WHERE last_name REGEXP '^my|se'
```

ð¥ REGEXP 'field$' = phrase ENDs w ...field

ð¤EXAMPLE ð§® last name ends w :   ey OR on 
```
SELECT *
FROM customers
WHERE last_name REGEXP 'ey$|on$'
```

ð¥ REGEXP 'field|Mac' = phrases HAS fiel OR mac â­ you can combine them 


ð¥ REGEXP '[ae]d' = HAS an A/E BEFORE D

ð¥ REGEXP 'd[ae]' = HAS an D after a/e

ð¤EXAMPLE ð§® contains :b  , followed by : r or u
```
SELECT *
FROM customers
WHERE last_name REGEXP 'b[ru]'
```
ð¥ REGEXP '[a-d]r' = HAS A--to--D AFTER R

- `IS NULL`  â¹ï¸ where column is empty

ð¤EXAMPLE ð§® orders that were not shipped
```
SELECT *
FROM orders
WHERE shipped_date IS NULL
```
--------------------------------------
# # ð¢ ORDER BY clause â¹ï¸ for sorting data for columns

ð¥ ORDER BY first_name DESC = descending
```
ORDER BY first_name
```
ð¤EXAMPLE ð§® sort by state descendent,  and if repeated, sort by name
```
SELECT *
FROM customers
ORDER BY state DESC, first_name
```
ð¤EXAMPLE ð§® calculate total_price Where order id = 2 â­
```
SELECT *, quantity * unit_price AS total_price
FROM order_items
WHERE order_id = 2
ORDER BY total_price DESC
```

-------------------------------------------
# # ð¢ LIMIT clause 

```
SELECT *
FROM customers
LIMIT 3
```
ð¥ LIMIT 6, 3 = offset

ð¤EXAMPLE ð§® dont show first 6, and then show 3 

should be showing 7-9
```
SELECT *
FROM customers
LIMIT 6, 3
```

ð¤EXAMPLEð§® get top 3 loyal customers (more points)
```
SELECT *
FROM customers
ORDER BY points DESC
LIMIT 3

```
-------------------------------------------
# # ð¢ INNER JOINS 
â¹ï¸ ðï¸ if you have same column  in multiple tables, you have to prefix them with the table_name.column

â¹ï¸ ðï¸ ALIAS : is like this > FROM orders o, so nxt time you write it you use **o** for orders

ð¤EXAMPLEð§®what im joining is the last 3 columns to order's table that its limited just to order_id
```
SELECT order_id, first_name, last_name,  c.customer_id
FROM orders o
JOIN customers c
	ON o.customer_id =  c.customer_id
```
âON o.customer_id =  c.customer_id ð

this  means we're saying that the foreing key in those 2 tables, will be the same in this NEW TABLE

ð¤EXAMPLEð§® join orders w products , for each order return pr id and name, followed by quantity and unit price from order item table
```
SELECT order_id, p.product_id, quantity, oi.unit_price
FROM order_items oi
JOIN products p ON 
	oi.product_id = p.product_id
```
-------------------------------------------
# # ð¢ JOINING ACROSS DATABASES 
ð¤EXAMPLEð§® combining columns from databases


```
SELECT *
FROM order_items oi
JOIN sql_inventory.products p
	ON oi.product_id = p.product_id
```
-------------------------------------------
# # ð¢ SELF JOINS
ð¤EXAMPLEð§® write the manager's name for every employer 

```
USE  sql_hr;

SELECT 
    e.employee_id,
    e.first_name,
    m.first_name AS manager 
FROM employees e
JOIN employees m 
	ON e.reports_to = m.employee_id 
```
-------------------------------------------
# # ð¢ MULTIPLE JOINS
ð§®here we re joining 3 tables 

ð JOIN customers c

âin o.customer_id i want -> c.customer_id

â¹ï¸ ðï¸ here we saying that foreign keys of both tables are the same 

ð JOIN order_statuses os 

âin o.status i want -> os.order_status_id
```
USE  sql_store;

SELECT 
    o.order_id, 
    o.order_date,
    c.first_name,
    c.last_name,
    os.name AS status
FROM orders o
JOIN customers c
	ON o.customer_id = c.customer_id
JOIN order_statuses os
	ON o.status = os.order_status_id
```
ð¤EXAMPLE ð§® join clients and join payment method to payment
```
USE  sql_invoicing;

SELECT 
	p.date,
    p.invoice_id,
    p.amount,
    c.name,
    pm.name
FROM payments p
JOIN clients c
	ON p.client_id = c.client_id
    -- it says foreing keys are the same
JOIN payment_methods pm
	ON p.payment_method = pm.payment_method_id
	-- we change the number for the payment name

```
â ON p.client_id = c.client_id 

â¹ï¸ ðï¸ here we saying that foreign keys of both tables are the same 
-------------------------------------------
# # ð¢ COMPOUND JOINS 
```
USE sql_store;

SELECT *
FROM order_items oi
JOIN order_item_notes oin
	ON oi.order_id = oin.order_Id
    AND oi.product_id = oin.product_id
    
```
-------------------------------------------
# # ð¢ â­a IMPLICIT JOIN SYNTAX
â­ ð§® slecting and joining everything in **FROM**, you change the **ON** with **WHERE** ð
```
USE sql_store;
SELECT *
FROM orders o, customers c
-- select & join in FROM
WHERE o.customer_id = c.customer_id
-- change the ON with WHERE
```
â the original for this will be ð

```
SELECT *
FROM orders o 
JOIN customers c
	ON o.cusmer_id = c.customer_id
```
-------------------------------------------
# # ð¢ OUTER JOINS// LEFT JOIN, OUTER J MULTiPLE, SELF OUTER J, 
ð§°`LEFT JOIN`

ð¤EXAMPLE ð§® all results are shown wether is empty or not 
```
USE sql_store;
SELECT p.product_id, p.name, oi.quantity
FROM products p
LEFT JOIN order_items oi
	ON p.product_id = oi.product_id
```
ð§° `outer join multiple tables`

ð¤EXAMPLE |  ð§®we join all orders to customers, and then join shipper id, then oin shipper name
```
USE sql_store;
SELECT 
	c.customer_id,
    c.first_name,
    o.order_id,
    sh.name AS shipper
FROM customers c
LEFT JOIN orders o
	ON c.customer_id = o.customer_id
LEFT JOIN shippers sh
	ON o.shipper_id = sh.shipper_id
ORDER BY c.customer_id
```
ð¤EXAMPLE 2 â­| ð§®orders, JOIN customers, view all shippers, JOIN status 
```
USE sql_store;

SELECT 
	o.order_date,
    o.order_id,
    c.first_name,
    sh.name AS shipper,
    -- to add sh column
    os.name AS status
FROM orders o 
JOIN customers c 
	ON o.customer_id = c.customer_id
    -- SYNC tables
LEFT JOIN shippers sh 
	ON o.shipper_id = sh.shipper_id
    -- SYNC tables ,left join to sshow all
JOIN order_statuses os
	ON o.status = os.order_status_id
```
ð§° `SELF OUTER JOIN`

ð¤EXð§® here we have employees and 1 of them is the manager so to see him in the list you should uuse LEFT JOIN

```
USE sql_hr;

SELECT 
	e.employee_id,
    e.first_name,
    m.first_name AS manager
FROM employees e
LEFT JOIN employees m
	ON e.reports_to = m.employee_id
ORDER BY manager
```
-------------------------------------------
# # ð¢ â­ USING clause 

ð§° `USING ()`

ðï¸ it only works if the name of column it's exactly the same 

â¹ï¸ it simplifys joining tables

original ð
```
USE sql_store;

SELECT*
FROM order_items oi
JOIN order_item_notes oin
	ON oi.order_id = oin.order_Id
    AND oi.product_id = oin.product_id
```
SIMPLIFIED W `USING`
```
USE sql_store;

SELECT*
FROM order_items oi
JOIN order_item_notes oin
	USING (order_Id, product_id)
    -- coma is an AND
```

ð¤EX ð§® complete using boths ON and USING 
```
USE sql_invoicing;

SELECT 
	p.date,
	c.name AS client,
    p.amount,
    pm.name AS payment_method
    -- here we bring the column we want
FROM payments p
JOIN clients c USING (client_id)
JOIN payment_methods pm
	ON p.payment_method = pm.payment_method_id
    -- here we put column id  to link them
```
-------------------------------------------
# # ð´ Union

â¹ï¸ here you combing rows from querys

ðï¸when UNION, number of columns should be equal otherwise you get an error

ð we select a new STRING that is not an actual column and assign it to a name 

ð¤EX 1 â­|`UNION`ð§® union w `<>=` conditions for point 'gold', 'bronze', 'silver' 
```
USE sql_store;

SELECT
	c.customer_id,
	first_name,
    c.points,
    'bronze' AS type
FROM customers c
WHERE points < 2000
UNION
SELECT
	c.customer_id,
	first_name,
    c.points,
    'silver' AS type
FROM customers c
WHERE points BETWEEN 2000 AND 3000
UNION
SELECT
	c.customer_id,
	first_name,
    c.points,
    'gold' AS type
FROM customers c
WHERE points > 3000
ORDER BY first_name

	

```

ð¤EX 2 | `UNION`ð§® you combine order statuses as 'actual' year and 'archieved'
```
USE sql_store;

SELECT 
	order_id,
    order_date,
    'active' AS status
FROM orders
WHERE order_date >= '2019-01-01'
--
UNION
--
SELECT 
	order_id,
    order_date,
    'archieved' AS status
FROM orders
WHERE order_date < '2019-01-01'

	

```
-----------------------------------------------------
# # ðµ INSERT a row into table 
â¹ï¸ `DEFAULT` = to generate an automatical ID that's not the same as others

-----------------------------------------------------
# # ð¢ INSERT MULTIPLES ROWS

ð¤EX ð§® inseting 3 products ...
```
INSERT INTO products (name, quantity_stock, unit_price)
VALUES ('product1', 10, 1.95),
	   ('product2', 12, 4.9),
       ('product3', 166, 1.95)
```
# # ð¢ INSERT HIERARCHICAL ROWS

â¹ï¸`SELECT LAST_INSERT_ID()` = function  
```
-- funtion
INSERT INTO order_items
VALUES
		(last_insert_id(), 1, 1 , 2.95),
		(LAST_INSERT_ID(),2,1,3.00)
```
--------------------------------------------------
# # ð¢ COPY TABLE , INSERTING IN TABLE W FILTERS
ðï¸ when creating a cipy like this , SQL ignores `Primary Key` & `AI(auto incrment prop)`
```
-- funtion
INSERT INTO order_items
VALUES
		(last_insert_id(), 1, 1 , 2.95),
		(LAST_INSERT_ID(),2,1,3.00)
```
ð `TRUNCATE TABLE config command` to delete everything from table

ð¤EX ð§® inserting orders placed before 2019 into orders_archieved that is a copy of orders table 
```
INSERT INTO orders_archieved 
SELECT * 
    
FROM orders
WHERE order_date < '2019-01-01'
```
â­ð§® creating a copy of records in invoices table, and put them in new table called invoices_archieved, and in cllient id we want client name (so join clients table ) using this as a subquery in the create statement. and only copy invioces that have a payment
```
USE sql_invoicing;

CREATE TABLE invoices_archieved AS
SELECT 
	i.invoice_id,
    i.number,
    c.name AS client,
    i.invoice_total,
    i.payment_total,
    i.invoice_date,
    i.payment_date,
    i.due_date
FROM invoices i 
JOIN clients c
	USING (client_id)
WHERE payment_date IS NOT NULL

```

-----------------------------------------------------------------------
# # ð¢ UPDATE SINGLE ROW
 `UPDATE` = table 
 
 `SET` = here we specify new value for 1 or more columns
 
 `WHERE` = here we specify the record or records that need to be udated
 ```
 USE sql_invoicing;

UPDATE invoices
SET 
	payment_total = 10,
    payment_date = '2019-03-01'
WHERE invoice_id = 1

 ```
# # ð¢ UPDATE MULTIPLE ROWS
 â¹ï¸ in th WHERE clause insted of = we use IN to specify more than 1 row
 
 ð§® +50 points to customers born before 1990
 ```
 USE sql_store;

UPDATE customers
SET 
	points = points + 50
WHERE birth_date < '1990-01-01'
 ```
 # # ð¢ UPDATE w subquerys
 ð§®update all clients located in NY or CA
 ```
 USE sql_invoicing;
UPDATE invoices
SET 
	payment_total = invoice_total * 0.5,
	payment_date = due_date
WHERE client_id = 
			(SELECT client_id
			FROM clients
			WHERE state IN ('NY','CA'))
 ```
```
USE sql_invoicing;
UPDATE invoices
SET 
	payment_total = invoice_total * 0.5,
    payment_date = due_date
WHERE payment_date IS NULL
```
 ð§® update orders commets where points more than 3000 points 
```
USE sql_store;

UPDATE orders
SET comments = 'Gold customer'
WHERE customer_id IN 
				(SELECT customer_id
				FROM customers c
				WHERE points > 3000)
```
--------------------------------------------------------------------------------------------------
# # ð´DELETING ROWS 
```
USE sql_invoicing;

DELETE FROM invoices
WHERE 	client_id = 	
			(SELECT *
            FROM clients
            WHERE name = 'Myworks')
```
-------------------------------------------------------------------------------------------------
# # ðµRESTORING DATA BASES
- `file`, `open SQL Script`

- 

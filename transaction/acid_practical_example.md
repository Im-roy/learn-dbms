# ACID examples

## Atomicity

Consider we have two tables.
- products
- sales

### products
|pid|name|price|inventory|
|---|----|-----|---------|
|1 | phone| 999.99 | 100
2 | laptop| 1200.50 | 50

### sales

saleid|pid|price|inventory|
|---|----|---------|----|
1| 1| 999.99 |12

products table contains details of products and sales table contain the amount od sold products.

Once is product is sold its inventory is reduced and simultaneously sales table gets updated.

```
postgres=# begin transaction
;
BEGIN
postgres=*# select * from products;
 pid | name  | price  | inventory 
-----+-------+--------+-----------
   1 | phone | 999.99 |       100
(1 row)

postgres=*# update products set inventory = inventory - 10;
UPDATE 1
postgres=*# select * from products;
 pid | name  | price  | inventory 
-----+-------+--------+-----------
   1 | phone | 999.99 |        90
(1 row)

postgres=*# select * from sales;
 saleid | pid | price | quantity 
--------+-----+-------+----------
(0 rows)

postgres=*# insert into sales (pid, price, quantity) values (1, 999.99, 10);
INSERT 0 1
postgres=*# select * from sales;
 saleid | pid | price  | quantity 
--------+-----+--------+----------
      1 |   1 | 999.99 |       10
(1 row)

postgres=*# commit;
COMMIT
```
All queries are part of one task thats why it is being executed in one transaction.

Now consider these senarios
### case 1. Database crashes while transaction is not complete

```
postgres=# begin transaction
;
BEGIN
postgres=*# select * from products;
 pid | name  | price  | inventory 
-----+-------+--------+-----------
   1 | phone | 999.99 |       100
(1 row)

postgres=*# update products set inventory = inventory - 10;
UPDATE 1
postgres=*# select * from products;
 pid | name  | price  | inventory 
-----+-------+--------+-----------
   1 | phone | 999.99 |        90
(1 row)

exit;
```
It we try to check the products table once database gets restored.

```
postgres=# begin transaction
;
BEGIN
postgres=*# select * from products;
 pid | name  | price  | inventory 
-----+-------+--------+-----------
   1 | phone | 999.99 |       100
(1 row)

postgres=*# update products set inventory = inventory - 10;
UPDATE 1
postgres=*# select * from products;
 pid | name  | price  | inventory 
-----+-------+--------+-----------
   1 | phone | 999.99 |        90
(1 row)
```
The amount of inventory also gets restored.

# Indexing

## Create million of rows in a table

```
postgres=# create table temp(t int);
CREATE TABLE

postgres=# insert into temp(t) select random()*100 from generate_series(0, 1000000);
INSERT 0 1000001
postgres=# select count(*) from temp;
  count  
---------
 1000001
(1 row)

postgres=# select t from temp limit 10;
 t  
----
 12
 45
 56
 42
 17
 15
  2
 20
 77
 74
(10 rows)

```
Indexing in Postgres is a way of creating a data structure that allows for faster search, sort and aggregate operations on a table or a column.

There are several types of indexes that can be created in Postgres, including:

- B-tree index: This is the default index type in Postgres, and it's used for most data types. It is efficient for searching and sorting large amounts of data.

- Hash index: This is used for equality comparisons of data types that have a hash function. It is efficient for small tables and for use cases that require fast lookups.

- GiST index: This is used for spatial data, and it allows for efficient search and retrieval of data based on its location.

- SP-GiST index: Similar to the GiST index, it is used for complex data types like IP addresses, text search, and ranges.

- GIN index: This is used for full-text search and retrieval of data based on its content.

- BRIN index: This is used for large tables with a sortable column, and it allows for fast retrieval of data based on its location within the table.

To create an index in Postgres, you can use the CREATE INDEX command. For example, to create a B-tree index on the "id" column of the "employees" table:

```
CREATE INDEX employees_id_idx ON employees(id);
```
It's important to note that creating an index also increases the amount of disk space used by the table, and that indexes should be carefully chosen based on the specific use cases and queries that will be run against the table.


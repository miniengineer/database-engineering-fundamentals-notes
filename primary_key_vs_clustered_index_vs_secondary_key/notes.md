Clustering - orginizing the table around a key.
Maintain order and rows that are inserted must follow the order == additional maintenance costs

### Primary key

- Can not be empty **NOT NULL**
- Value must be unique **no duplicates**
- By default no specific order, a way to uniquely identify a row in a table
- Don't use random values - impossible to orginize by
- DB usually cluster tables by primary key
- The fact that rows are orginized by primary key increases speed of IOs
- Primary key != Clustered index (e.g. Postrgres)

### Clustered index

- Good at finding specific rows in a table
- Rows in the table are physically ordered by clustered index
- Common practice is to apply primary key and clustered index constraint to the same column
- Can be only one clustered index per table
- Accessing a row through the clustered index is fast because the index search leads directly to the page in Heap that contains the row data

### Secondary key

- A secondary value that is unique for each record used to identify the record
- A potential primary key by which table can be indexed (e.g. email column in users table)
- Possbiel to maintain a separate ordered structure of secondary keys while keeping the table unordered
- IO into the index table, find row ids and, go to the Heap(table) to fetch actual data

### DBs

## Postgres

Primary indexes are not clustered in Postqres, it applies clustering to the secondary key - id.
A **CLUSTER** command must be used to add clustering and reorganize the table.

PostgreSQL automatically creates indexes for **PRIMARY KEY** and every **UNIQUE** constraints of a table.

## MySQL

If a table column contains a **PRIMARY KEY**, MySQL automatically creates a clustered index named **PRIMARY**.
If **PRIMARY KEY** is not defined for a table, MySQL uses the first UNIQUE index with all key columns defined as **NOT NULL** as the clustered index.

If none of the above-mentioned is present in the table, MySQL will generate a hidden clustered index.

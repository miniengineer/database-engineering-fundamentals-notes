## DB internals

- Table
- Row_id
- Page
- IO
- Heap DS
- Index DS b-tree

### Table

A collection of related data in a table format - rows and columns

### Row_id

- Internal and system maintained
- In some DBs(mysql, InnoDB) same as primary key, some created their own internally managed column row_id (Postgres)

### Page

Rows are stored in pages - fixed sized memory location of raw table data (bytes).

- IO doesn't read a single row, it reads a page or more
- Page has a fixed (configurable) size, has start and end
- In general contains several rows, some might stretch for more than one page
- Row_id is a pointer to a page where the row is located (e.g. not like RAM where byte address points byte exact location)

Number of pages pulled and IOs made affects the performance.

### IO

- Operation (input/output) is a read request to the disk (heap)
- Try to do as few as possible since they are expensive
- Can fetch one or more pages, depending on disk partitioning and other settings
- Can't read a single row, has to fetch a page or more with many rows in them
- Some go to the OS cache, not disk

### Heap DS

- Data structure where the table data is actually stored in pages
- Fetching data from heap is expensive as we will read a lot of unnecessary data
- Indexes help with indenfying which page of the heap we need to pull

### Index DS

- Data structure that has a part of data and a pointer to the data location in the heap
- Helps to quickly search data
- Index can be added to one or more columns
- Once you find the index, you can use it to search for a page in the heap where data is located
- Index tells **exactly** which page to fetch in the heap instead of scanning each page (index scan vs sequential scan)
- Index itself is also stored in pages and takes IOs to fetch
- The smaller the index, the more it can fit in memory the faster the search
- A popular DS for indexes is **b-tree** (similar to binary search tree)

For example, if there are no indexes on a table and we execute the following query for the first time

```
select * from customers where customer_id = 777;
```

DB will have to go straight to the Heap and scan all the pages - perform a sequential scan until it finds the `customer` with `customer_id` of 777.
Some DBs do parallel scan from top & bottom to optimize, but it's still an expensive query.

In case we added index on `customer_id` column, DB goes to the b-tree first until it finds the exact address of the page and row number.
Only then it makes the IO to the Heap and pulls the necessary row.

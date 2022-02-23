### What is index?

A data structure that is build and assigned on top of the existing table to create shortcuts to the table.
Think about a phone book which is labeled with letters that allows searching for a specific entry much faster.

Every primary key have an index by default and it's stored in btree data structure.

_Example_

employees table with 10,000,000 rows

- `id` is a sequential primary key
- `name` is a string

| id  | name   |
| --- | ------ |
| 1   | Minira |
| 2   | Mark   |
| 3   | James  |
| ..  | ..     |

## Select id

```
SELECT id FROM employees WHERE id = 7000;
```

Will result in **index only scan**, since `id` has an index the query will go into b-tree (similarity with binary search) and return `id`. Since `id` is the only selection there is no need to fetch the Heap.

## Select name

```
SELECT name FROM employees WHERE id = 5000;
```

Will result in **index scan** which is slower.
We have an `id` which will result in b-tree search that will give location of `name` in the Heap.
We will still need to fetch the Heap to retrieve `name`.

Query becomes faster after caching.

## Select id, with condition on name

```
SELECT id FROM employees WHERE name = 'greg';
```

Will result in **(parallel) sequential scan** (scan is shared amonth miltiple processes) of the Heap since `name` column doesn't have index. Need to go through all rows (pages) in the Heap till it finds the match - execute full table scan until row where `name` is `greg` is found.

## Select id, with wildcard condition on name

```
SELECT id FROM employees WHERE name LIKE '%ni%';
```

Very expensive query, since full table scan is executed and all matching rows will be fetched.
Not searching for a certain value, searching for an expression.
Even if index is added to `name` column, there can't be index that can satisfy it since it's not a single value!

### Conclusion

Primary key always has index.
Indexes are stored in b-tree data structure which allow fast search.

- Index Only Scan - no Heap fetch => fastest
- Index Scan - fetch concrete location (page) in Heap => fast
- Sequential Scan - search in each page in Heap => slow

Try to avoid fetching the Heap as much as possible.
Add indexing to columns that you're gonna fetch regularly, but don't forget that indexing everything means that more information must be stored in memory.

Having an index doesn't mean that DB will always use it, e.g. queries with wildcard (`*, %`) conditions.

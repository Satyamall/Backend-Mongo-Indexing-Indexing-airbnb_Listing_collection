
# using the airbnb listing collection for Mongo Indexing

**index the price field**

```js
 {price: {$gte: 4000}}
```
the time difference between the queries:
Actual Query Execution Time (ms):0
FETCH - Execution time 0ms.
IXSCAN - Execution time 0ms.


 
 ```js
   {price: {$lte: 4000}}
 ```
the time difference between the queries:
Actual Query Execution Time (ms):12
FETCH - Execution time 0ms.
IXSCAN - Execution time 1ms.


```js
{price: {$in: [4103,5260,6400]}}
```
the time difference between the queries:
Actual Query Execution Time (ms):0
FETCH - Execution time 0ms.
IXSCAN - Execution time 0ms.

**index the address.country field**

```js
{price: {$gte: 5000}, 'address.country': 'Canada' }
```
the time difference between the queries:
Actual Query Execution Time (ms):0
FETCH - Execution time 0ms.
IXSCAN - Execution time 0ms.

Query used the following index:
price

```js
{'address.country': 'Canada' }
```
the time difference between the queries:
Actual Query Execution Time (ms):1
FETCH - Execution time 0ms.
IXSCAN - Execution time 0ms.

Query used the following index:
address.country


```js
{'address.country': 'Canada', price: {$gte: 4000} }
```
the time difference between the queries:
Actual Query Execution Time (ms):0
FETCH - Execution time 0ms.
IXSCAN - Execution time 0ms.

Query used the following index:
address.price

# part 2

**Compound index**

# explain pros and cons of compound index.
 
 **Advantages:**
 - A composite index provides opportunities for index covering.
 - If queries provide search arguments on each of the keys, the compound index requires fewer I/Os than the same query using an index on any single attribute.
 - A compound index is a good way to enforce the uniqueness of multiple attributes.

**Good choices for composite indexes are:**
 - Lookup tables
 - Columns that are frequently accessed together
 - Columns used for vector aggregates
 - Columns that make a frequently used subset from a table with very wide rows

**Disadvantages of compound indexes are:**
 - Composite indexes tend to have large entries. This means fewer index entries per index page and more index pages to read.
 - An update to any attribute of a composite index causes the index to be modified. The columns you choose should not be those that are updated often.

**Poor choices are:**
 - Indexes that are nearly as wide as the table
 - Composite indexes where only a minor key is used in the where clause

# Explain prefix requirements for compound index

If the sort keys correspond to the index keys or an index prefix, MongoDB can use the index to sort the query results. A prefix of a compound index is a subset that consists of one or more keys at the start of the index key pattern.

# when does compound index not work?

Compound Index not working in Spring Data MongoDB

or

When evaluating the clauses in the $gte expression, MongoDB either performs a collection scan or, if all the clauses are supported by indexes, MongoDB performs index scans. That is, for MongoDB to use indexes to evaluate an $gte expression, all the clauses in the $gte expression must be supported by indexes. Otherwise, MongoDB will perform a collection scan.

```js
{property_type: "House"}
```

# compound Index working like these type of queries

```js
{price: {$gte: 4000}}
```

# apply compound index on 3 fields that are commonly searched in airbnb, and query those fields and sort them to see explain
```js
{price: {$gte: 4000}, property_type: "House", "address.country": "Brazil"}
```


```js
{price: {$gte: 4000}, "address.country": "Brazil"}

{price: 1, property_type: -1}
```


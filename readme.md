
# using the airbnb listing collection for Mongo Indexing

**index the price field**

```js
 {price: {$gte: 4000}}
```
the time difference between the queries:
Actual Query Execution Time (ms):0
FETCH - Execution time 0ms.
IXSCAN - Execution time 0ms.
![Screenshot (300)](https://user-images.githubusercontent.com/80479635/157243422-43cc5d59-91cc-40d3-b3c1-04f5dbbb14fb.png)


 
 ```js
   {price: {$lte: 4000}}
 ```
the time difference between the queries:
Actual Query Execution Time (ms):12
FETCH - Execution time 0ms.
IXSCAN - Execution time 1ms.

![Screenshot (301)](https://user-images.githubusercontent.com/80479635/157243437-916ea108-aac9-4dd0-ad27-9b4ff78f21bc.png)

```js
{price: {$in: [4103,5260,6400]}}
```
the time difference between the queries:
Actual Query Execution Time (ms):0
FETCH - Execution time 0ms.
IXSCAN - Execution time 0ms.
![Screenshot (302)](https://user-images.githubusercontent.com/80479635/157243477-1b5a3334-8644-40d4-b15b-75316f884961.png)

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
![Screenshot (303)](https://user-images.githubusercontent.com/80479635/157243512-92b13645-e239-401a-8ac3-9241ef16fc91.png)


```js
{'address.country': 'Canada' }
```
the time difference between the queries:
Actual Query Execution Time (ms):1
FETCH - Execution time 0ms.
IXSCAN - Execution time 0ms.

Query used the following index:
address.country
![Screenshot (305)](https://user-images.githubusercontent.com/80479635/157243603-98ec2bb1-bab0-4740-934a-c529507206c3.png)


```js
{'address.country': 'Canada', price: {$gte: 4000} }
```
the time difference between the queries:
Actual Query Execution Time (ms):0
FETCH - Execution time 0ms.
IXSCAN - Execution time 0ms.

Query used the following index:
address.price
![Screenshot (304)](https://user-images.githubusercontent.com/80479635/157243579-94ae5bae-c80a-443a-b64c-a7cd198fdc5a.png)

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
![Screenshot (306)](https://user-images.githubusercontent.com/80479635/157243679-b5df4a36-2865-4a46-9229-d0f9c2f580cd.png)

# compound Index working like these type of queries

```js
{price: {$gte: 4000}}
```
![Screenshot (307)](https://user-images.githubusercontent.com/80479635/157243707-3b7d6856-a43d-4b9a-a765-c1249456cbf4.png)

# apply compound index on 3 fields that are commonly searched in airbnb, and query those fields and sort them to see explain
```js
{price: {$gte: 4000}, property_type: "House", "address.country": "Brazil"}
```
![Screenshot (308)](https://user-images.githubusercontent.com/80479635/157243737-fabe3bbd-4516-40fc-9321-7bccf7eaacc0.png)


```js
{price: {$gte: 4000}, "address.country": "Brazil"}

{price: 1, property_type: -1}
```
![Screenshot (309)](https://user-images.githubusercontent.com/80479635/157243774-1ce2e897-42d3-4ab5-8977-d90ccf3ec19f.png)
![Screenshot (310)](https://user-images.githubusercontent.com/80479635/157243835-9ad4dfb4-abcf-4ff6-a286-697648c41de2.png)
![Screenshot (311)](https://user-images.githubusercontent.com/80479635/157243853-7c205dfe-5f51-467b-ba9a-c1573a98d721.png)
![Screenshot (312)](https://user-images.githubusercontent.com/80479635/157243872-439b0202-a926-45da-a57e-682b80611582.png)


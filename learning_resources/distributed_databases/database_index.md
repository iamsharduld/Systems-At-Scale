# Database Indexes

Database indexes are data structures that improve the speed of data retrieval operations on a database. They provide a lookup table for the database engine to quickly locate the data associated with a search key. Without an index, the database engine must go through every row in a table to find the relevant rows, which can be slow if there are a large number of rows.

## 1. Structure

- [**B-Tree Index**](#b-tree-index): Most relational databases use a B-tree or a variant of it (such as B+ tree) for indexes. The B-tree data structure allows the database engine to quickly navigate through keys and find the row or rows associated with a key.

- **Hash Index**: Some databases use hash indexes, where a hash function transforms the key value into a location in an array.

- **Bitmap Index**: Bitmap indexes use bitmaps and are useful for columns with a limited number of unique values (e.g., gender, true/false flags).

## 2. Types of Indexes

- **Primary Index**: Typically used on the primary key of a table, ensuring uniqueness and fast access.

- **Secondary Index**: Used on non-primary key columns to enable faster searches on those columns.

- **Composite Index**: An index that includes more than one column, useful for queries that filter on multiple columns.

- **Covering Index**: Contains all the columns needed to process a particular query, so the database can retrieve the data directly from the index without accessing the table.

- **Full-Text Index**: Specialized index used to enable text search within a large string or collection of strings.

## 3. How They Work

Suppose you have a table with millions of rows and you want to find all the rows where the "username" column is equal to "john_doe". Without an index on the "username" column, the database would have to scan the entire table row by row.

With an index on the "username" column, the database can quickly find the location of the data for "john_doe" by traversing the B-tree or other index structure. This significantly reduces the time needed to find the relevant data.

## 4. Trade-offs

- **Speed vs Space**: Indexes can dramatically speed up data retrieval but consume additional disk space.

- **Write Performance**: Maintaining indexes requires extra work during insert, update, or delete operations, which can slow down write performance.

- **Maintenance**: Indexes can become fragmented over time, requiring periodic maintenance to keep them optimized.

## 5. When to Use

Indexes should be used judiciously, especially in columns that are frequently searched or sorted. Analyzing the queries that your application uses most often can help you decide where indexes are needed.

## Index Data Structure

### B-tree Index

A B-tree index is a specific type of data structure used in databases to improve the speed of search operations. It is commonly used to index large datasets and provides efficient access to both range queries and individual lookups.

#### Structure

A B-tree (Balanced Tree) is a self-balancing tree structure that maintains sorted data in a way that allows searches, insertions, and deletions to be carried out efficiently. The B-tree has the following properties:

1. **Nodes**: The tree is made up of nodes, where each node contains a certain number of keys and child pointers.
2. **Order**: The "order" of a B-tree (often denoted as `m`) refers to the maximum number of children a node can have. The minimum number of keys in a non-root node is `ceil(m/2) - 1`.
3. **Root**: The root node may contain as few as one key.
4. **Leaves**: All leaf nodes are at the same depth, and they carry no information about children (i.e., they have no child pointers).
5. **Keys**: The keys within a node are sorted, and they divide the keys in the subtrees: if `k` is a key in a node `N` with children `C1, C2,...,Cm`, then all the keys in `C1` are less than `k`, and all the keys in `C2` are greater than `k`.

#### Operations

1. **Search**: Searching within a B-tree index is efficient, usually taking O(log n) time, where n is the number of keys. The search begins at the root and follows the child pointers, comparing the search key with the keys in the current node, until it either finds the key or reaches a leaf node without finding it.
2. **Insert**: Inserting a new key also takes O(log n) time. The algorithm finds the correct leaf node where the new key should reside and inserts it there. If that node is now too large, it splits into two nodes, and this splitting may propagate up the tree, possibly leading to the creation of a new root.
3. **Delete**: Deletion takes O(log n) time as well. The key is first located, and if it's in an internal node, it's replaced with its in-order predecessor or successor (which must reside in a leaf). Then, the key in the leaf is deleted. If this makes the leaf too small, keys are redistributed from neighboring nodes or nodes are merged, possibly propagating changes up the tree.

#### Use in Databases

In the context of a relational database, a B-tree index is commonly used on columns where range queries or sorted results are needed. The keys in the B-tree are the values from the indexed column, and the associated values are typically pointers to the rows in the database having those key values.

#### Conclusion

The B-tree index is a powerful and flexible tool in the efficient management of large data sets. By maintaining a balanced structure, it ensures that basic operations like search, insert, and delete all occur in logarithmic time. This efficiency makes B-tree indexes widely used in various database systems.

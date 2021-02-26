# Techinical Article Notes

**Links**
* https://medium.com/@shashankbaravani/database-storage-engines-under-the-hood-705418dc0e35
* https://hub.packtpub.com/how-sql-server-handles-data-under-the-hood/
* https://en.wikipedia.org/wiki/B%2B_tree
* https://towardsdatascience.com/how-b-tree-indexes-are-built-in-a-database-6f847fb3cfcc?gi=fc3b3159bbf4
* https://cstack.github.io/db_tutorial/
* https://dzone.com/articles/database-btree-indexing-in-sqlite
* https://medium.com/basecs/busying-oneself-with-b-trees-78bbf10522e7

"What I cannot create, I do not understand." – Richard Feynman
"We are so much the victims of abstraction that with the Earth in flames we can barely rouse ourselves to wander across the room and look at the thermostat." - Terrance McKenna

**What is a B-Tree?**

A b-tree "Bee Tree" is a m-ary tree data structure that stores keys in its nodes in a sorted, ascending order.
Each key has two references to two child node.
The left side child node keys are less than the current keys and the right side child node keys are greater than the current keys. If a single node has *n* number of keys, then it can have a maximum *n+1* child nodes.

**What is a M-ary Tree?**

A m-ary tree is a rooted tree in which each node has no more than *m* children.
This is different than a binary tree where m=2, and a ternary tree where m=3.
Both a binary tree and a ternary tree are m-ary trees.

**Time Complexity**

| Type           | Insert   | Delete   | Search   |
| -------------- | -------- | -------- | -------- |
| Unsorted Array | O(1)     | O(n)     | O(n)     |
| Sorted Array   | O(n)     | O(n)     | O(log n) |
| B-Tree         | O(log n) | O(log n) | O(log n) |

**What is a B+Tree?**
A B+Tree is similar to a B-Tree.
However a B+Tree only stores data on it's leaf nodes.
All non-leaf nodes are duplicated as leaf nodes.
Additionally, you can travel through all the values in a leaf node with pointers.

**How does a B+Tree store key-values?**
The database first creates a primary key and converts the rows into a bytestream.
It stores each of the keys and records the bytestream on a B+Tree.
The primary key is used as they key for indexing.
A corresponding key and bytestream is known as a **payload**.

Say Name, Age, and FavColor needs to be stored in a database.
Each payload is stored in a B+Tree.
With a primary key, the database creates three B-Trees, one for each column in the table.
The value for each column is they key while the primary key is the value.
To perform the search

```SQL
SELECT * FROM users WHERE name=Max
```

The database first looks up the given key (Max) in the corresponding B-Tree and retrieves the primary key in O(log n) time.
The it uses the primary key to search the B+Tree using the primary key and retrieves the whole record.

**What is a byte stream?**
A bytestream is a sequence of bytes (1s and 0s).

**How are the nodes stored?**
Each node is stored inside the Pages. Pages are fixed in size.
Pages have a unique number starting at 1.
A page can be a reference to another page by using the same page number.
The page stores meta details such as the rightmost child page number, first free cell, offset, and first cell offset stored. There can be two types of pages in databases:
1. Pages for indexing: These pages store an index and a reference to the next page
2. Pages to store records: These pages are leaf pages thart store the actual data

B-Trees are used to store index.
B+Trees are used to store tables.


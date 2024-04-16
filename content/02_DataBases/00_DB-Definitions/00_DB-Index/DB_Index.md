#DataBases 

---
## # Also relevant
- [[DB_Trees]]

---

### # What is an Index? 

An index is a <mark style="background: #D2B3FFA6;">data structure</mark> that <mark style="background: #D2B3FFA6;">improves the speed of data retrieval operations</mark> on a database table. 
Instead of iterating through a whole table in a DB, the <mark style="background: #D2B3FFA6;">DB searches for an index</mark> - to find the position, where the data is stored.
It works similar to an table of contents in a book, providing a <mark style="background: #D2B3FFA6;">quick reference to the location of data</mark> based on specific columns.

---
### # Why Do I Need an Index?

Improves DataBase performance - especially when dealing with large datasets.
Increase efficiency of a DataBase.

**- Advantages of Using Index:**

- Faster Search Operations: Indexes accelerate search operations.
- Improved Query Performance: Indexes can enhance the performance of queries.

**- Disadvantages of Using Index:**

- Increased Storage Space: Indexes consume additional storage space in the database.
- Update Costs: Frequent updates, inserts, or deletes in the table can lead to increased maintenance costs for indexes.
- Overhead in Insert Operations: Adding indexes may impact the performance of insert operations due to the need for index updates.

---
### What are Index Structures?

Indexes are often implemented using tree structures
such as:
- B-Tree
- R-Tree
- ...

The details for Trees - [[DB_Trees]]

---





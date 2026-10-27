A better understanding and a more general solution to deal with fan trap issues is to use the **Unified Star Schema (USS)**, as explained in the book *The Unified Star Schema* by Francesco Puppini.

# Chasm trap

The **chasm trap** is another very common problem that we face in reporting.

In essence, a chasm trap is an unmanaged and often unintentional many-to-many relationship.

Because it is unmanaged, it becomes an unintended partial Cartesian product.

![](bbox=[0.12, 0.295, 0.88, 0.47])

Figure 3.21: Chasm trap and a correct many-to-many relationship model

The previous figure illustrates *the scenario for chasm trap: having two tables with a common key that is not unique in both tables.* Tables A and B both have an FK toward table X, so the key appears multiple times on each table. Please note that table X is not needed for the chasm trap problem to exist. It is there to show that tables A and B have a key in common and also because the “shared dimension” is the most common case where we immediately see a chasm trap.

In a chasm trap scenario, instead of a full Cartesian product, you will have a key-constrained Cartesian product: the rows on one side will be cross-joined with the occurrences of the same key on the other side, de facto making a per-key Cartesian product.

The difference between a chasm trap and a real many-to-many relationship is that in the latter you have the mapping table that limits the Cartesian product to only those occurrences that make sense. In the many-to-many case, you enter the mapping with one key, you have the designated M rows that give the actual mapping, and then you move to the other table where you only have one row, so in the end, you have only as many rows as there are in the mapping table. This make sense because the mapping table is built to represent a specific relationship.

In the chasm trap case, you enter the mapping table with M rows matching a key in one table and you find N rows with the same key in the other table, in the end producing MxN rows joined on that key, but you have no actual relation between the rows of the two tables as the common FK is there to express a relation from the two tables toward the table that has that key as a PK, not to express a relation between these two tables.
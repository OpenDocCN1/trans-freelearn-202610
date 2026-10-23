# Indexes

An index is a data structure that allows faster access to the underlying table so that specific tuples can be found quickly. Here, “quickly” means faster than scanning the whole underlying table and analyzing every single tuple.

PostgreSQL supports different types of indexes, and not all types are optimal for every scenario and workload. In the following sections, you will discover the main types of indexes that PostgreSQL provides, but in any case, you can extend PostgreSQL with your own indexes or indexes provided by extensions.

An index in PostgreSQL can be built on a single column or multiple columns at once; PostgreSQL supports indexes with up to 32 columns.

An index can cover all the data in the underlying table, or can index specific values only – in that case, the index is known as “partial.” For example, you can decide to index only those values of certain columns that you are going to use the most.

An index can also be unique, meaning that it is used to ensure the uniqueness of the values it indexes, such as, for example, the primary keys of a table. Moreover, an index can be built on top of a user-defined function, which means the index is going to index the return values of those functions.

> In order to be used in an index, a user-defined function must be declared as IMMUTABLE, which means its output must be the same for the very same input.

PostgreSQL is able to mix and match indexes together; therefore, multiple different indexes can be used to satisfy the query plan. Thanks to this important feature of PostgreSQL, you don’t have to define all the possible column permutation indexes, since PostgreSQL will try to mix unrelated indexes together.

In the following subsections, you will learn about all the available indexes types in a PostgreSQL 16 cluster, as well as how to create or drop an index.

# Index types

The default index PostgreSQL uses is Balanced Tree (B-Tree), a particular implementation of a tree that keeps its depth constant even with large increases in the size of the underlying table, therefore requiring the same effort to be traversed from the root to its leaves.
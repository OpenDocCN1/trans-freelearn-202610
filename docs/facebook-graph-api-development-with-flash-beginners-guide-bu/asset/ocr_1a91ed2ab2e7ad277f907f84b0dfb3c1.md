> Note that this restriction only requires *at least one* of the fields queried to be indexable. The fields specified in the `SELECT` clause can be any fields in the table specified in the `FROM` clause. And later in the chapter, you'll see how to query multiple fields at once inside a single `WHERE` clause, only one of which needs to be indexable.

# Does this matter in practice?

In general, the fields that you actually need to use in a search query are indexable; it's rare that you'll want to search for a user based on the URL of their profile picture. There are exceptions to watch out for, though. For instance, in the `event` table, only the `eid` field (the primary key) is indexable, meaning that you cannot search across all events in the database to find one taking place in a specified location, or containing a specified term in its name.

# Advanced FQL

What we've covered so far is enough to cover most of what you can do with Graph calls, and some more besides. Let's take a look at the more advanced features of FQL.

# Operators

Just like `if` statements in AS3, FQL's `WHERE` clause can take both logical and comparison operators.

# Comparison

We can use the mathematical comparison operators =, <, >, <=, >=, and !=. (An alternative to != is <>, which you may see in examples online.)

These all work the same as in AS3, with the exception of =, which is like AS3's ==; it's used for checking equality rather than assigning a value.

The most useful scenario for using these is when you want to filter out objects past a certain age.

# Logical

`AND`, `OR`, and `NOT` will be familiar to you, although in AS3 we would write them as `&&`, `||`, and `!`, respectively.

We already covered `IN` earlier in the chapter; this allows us to check whether a given item exists in a given list.
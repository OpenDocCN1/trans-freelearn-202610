# Literals

There are a couple of minor additional capabilities and readability enhancements when specifying literal values in code. You can specify binary literals, which means you don't have to work out how to represent them using a different base anymore. You can also put underscores anywhere within a literal to make it easier to read the number. The underscores are ignored but allow you to separate digits into convention groupings. This is particularly well suited to the new binary literal as it can be very verbose, listing out all those zeros and ones.

Take the following example that uses the new 0b prefix to specify a binary literal that will be rendered as an integer in a string:

```
Console.WriteLine($"Binary solo! {0b0000001_00000011_000000111_00001111}");
```

You can do this with other bases too, such as this integer, which is formatted to use a thousands separator:

```
Console.WriteLine($"Over {9_000:#,0}!"); // Prints "Over 9,000!"
```

# Tuples

One of the big new features in C# 7 is support for tuples. Tuples are groups of values, and you can now return them directly from method calls. You are no longer restricted to returning a single value. Previously, you could work around this limitation in a few suboptimal ways, including creating a custom complex object to return, perhaps with a Plain Old C# Object (POCO) or Data Transfer Object (DTO), which are the same thing. You could have also passed in a reference using the ref or out keyword, which are still not great although there are improvements to the syntax.

There was System.Tuple in C# 6, but it wasn't ideal. It was a framework feature, rather than a language feature, and the items were only numbered and not named. With C# 7 tuples, you can name the objects and they make a great alternative to anonymous types, particularly in LINQ query expression lambda functions. As an example, if you only want to work on a subset of the data available, perhaps when filtering a database table with an O/RM, such as Entity Framework, then you could use a tuple for this.
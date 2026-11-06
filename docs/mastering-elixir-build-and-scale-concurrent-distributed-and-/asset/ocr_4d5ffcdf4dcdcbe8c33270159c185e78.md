# Atoms

*Atoms* are a constant, whose value is its own name. They are always prefixed with a leading colon (:), followed by alphanumeric characters (and possibly _ or @). They may terminate with an exclamation or a question mark. Atoms are similar to enumerations in C and symbols in Ruby. Here are some examples of atoms:

```
iex> :ok
:ok
iex> :error
:error
iex> :some_descriptive_name!
:some_descriptive_name!
iex> :value@start
:value@start
```

You can create atoms with arbitrary characters with the following syntax:

```
iex> :"Atom name with arbitrary characters#$%^"
:"Atom name with arbitrary characters#$%^"
```

As with all data structures in Elixir, atoms can't be modified after their creation. Furthermore, they are not garbage-collected. Atoms are kept in the *atom table*, and upon compilation, their value is replaced by a reference to their entry on this table. This makes comparing atoms very efficient. As you'll learn throughout this book, this is one of the major use cases for atoms in Elixir, as we are constantly matching the return of a function against a certain expected atom.

![](bbox=[0.1, 0.6, 0.2, 0.7])

Since atoms are not garbage collected, don't create atoms dynamically from sources you can't control, as you can very easily use up all of the space allocated for the atom table. For instance, if you're parsing a JSON response and creating a map out of it, don't use atoms for its keys—use strings instead (both of these types, maps and strings, will be described later in this chapter).

# Booleans

Elixir has three values related to Boolean operations: `true`, `false`, and `nil` (where `nil` represents the absence of value—similar to `null` in most other languages). However, those are just some syntactic sugar, as internally they are represented as atoms of the same name, as you can see in the following example:

```
iex> true == :true
```
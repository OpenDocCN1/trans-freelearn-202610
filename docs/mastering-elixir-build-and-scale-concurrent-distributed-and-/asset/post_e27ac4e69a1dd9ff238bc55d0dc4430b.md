
## Tuples

*Tuples* are used to group a fixed number of elements together. They can hold any value—even other tuples. They are stored contiguously in memory, which provides constant access time to elements inside a tuple. You create a tuple surrounding the elements with curly brackets ({ and }), and separate the elements with commas:

```
iex> {:ok, 3.14}
{:ok, 3.14}
```

A common usage of tuples in Elixir is to pattern-match on the result of a function to ensure its success (usually with an :ok atom) or deal with an error. We will be looking to pattern matching and functions later in this chapter.

To access an element inside a tuple, we use the elem function (from the Kernel module), providing the tuple and a zero-based index:

```
iex> result = {:ok, 3.14}
{:ok, 3.14}
iex> elem(result, 1)
3.14
```

![](img/c636f4cfb07f25f1db535995e50706ed_26_0.png)

Functions from the Kernel module are auto-imported. Thus, we don't need to prefix them with the module name.

To change the elements on a tuple, you can use the put_elem function. The arguments are similar to the elem function, but you also provide the new value for that position of the tuple:

```
iex> put_elem(result, 1, 1.61)
{:ok, 1.61}
iex> result
{:ok, 3.14}
```

Notice how the result variable hasn't changed. As we discussed in the beginning of this chapter, data in Elixir is immutable. As such, although we've updated the tuple with a new value, the original tuple hasn't changed—Elixir updated the value on a copy of the original tuple. This way our code is side-effect free, and any other function holding a reference to the result variable won't have any surprises.

The general recommendation in Elixir is that tuples should hold up to four elements—anything more than that and you probably should be using another type.

## Lists

Lists are created by wrapping the elements we want inside it with square brackets ([ and ]), separating the values with commas. Internally, lists are implemented as singly linked lists, meaning that accessing the elements of a list is a O(n) operation. Lists aren't stored contiguously in memory as arrays in other languages. As with tuples, list elements can be of any type:

```
iex> [1, :an_atom, 0.5]
[1, :an_atom, 0.5]
```

We have the ++ and -- operators that are exclusive to lists, and serve to concatenate and subtract lists, respectively:

```
iex> [0, 1, 1] ++ [2, 3, 5]
[0, 1, 1, 2, 3, 5]
iex> [0, 1, 1] -- [1, 2, 3]
[0, 1]
```

To check whether a certain element is present in a list, you can use the in operator:

```
iex> 1 in [0, 1, 1, 2, 3, 5]
true
iex> 99 in [0, 1, 1, 2, 3, 5]
false
```

To get the head of a list, we use the hd function, whereas to get the tail of a list, we use the tl function:

```
iex> hd([0, 1, 1, 2, 3, 5])
0
iex> tl([0, 1, 1, 2, 3, 5])
[1, 1, 2, 3, 5]
```

Notice that the semantic of tail here is the list without its head (which is also a list), and not the last element of a list. We'll be exploring this concept in more depth, along with some more examples on how to work with lists, in the Working with collections section. For reference, you can find a detailed list of operations you can make on lists at https://hexdocs.pm/elixir/List.html.

![](img/c636f4cfb07f25f1db535995e50706ed_27_0.png)

Appending to a list is a O(n) operation, as we need to traverse the whole list. Prepending to a list is O(1). To prepend an element to a list, you can use the following syntax: [new_element | list].

## Maps

Maps are key-value data structures, where both the key and the value can be of any type. They're similar to hashes in Ruby and dictionaries in Python. To create a map, you enclose your key-value pairs in %{ }, and put a => between the key and the value, as we can see in the following snippet:

```
iex> %{ :name => "Gabriel", :age => 1}
%{age: 1, name: "Gabriel"}
```

In this case, the keys are both of the same type, but this isn't required. If your keys are atoms, you can use the following syntax to make the map declaration simpler:

```
iex> %{name: "Gabriel", age: 1}
%{age: 1, name: "Gabriel"}
```

To access the value associated with a certain key, put the key inside square brackets in front of the map:

```
iex> map = %{name: "Gabriel", age: 1}
%{age: 1, name: "Gabriel"}
iex> map[:name]
"Gabriel"
```

As with the map declaration, when the key is an atom, we have some syntactic sugar on top of it:

```
iex> map.name
"Gabriel"
```

![](img/c636f4cfb07f25f1db535995e50706ed_28_0.png)

When you try to fetch a key that doesn't exist in the map, a KeyError error will be raised when using the map.key syntax—unlike the map[key] syntax, which will return nil.

To update a key in a map, you can use %{map | key => new_value}. If the key is an atom, we can use the same notation described previously:

```
iex> %{map | age: 2}
%{age: 2, name: "Gabriel"}
```

![](img/c636f4cfb07f25f1db535995e50706ed_28_1.png)

If you're coming from an object-oriented programming background, you may instinctively use the following syntax to change the value of a key: map[key] = new_value. Remember that in Elixir all types are immutable and you never operate on the data structure itself but always on a copy of it.

This will only work for keys that already exist in the map—this constraint allows Elixir to optimize and reuse the fields list when updating a map. If you want to insert a new key, use the put function from the Map module:

```
iex> Map.put(map, :gender, "Male")
%{age: 1, gender: "Male", name: "Gabriel"}
```

As with all other types, in the official documentation, at https://hexdocs.pm/elixir/Map.html, you can find a pretty detailed reference on what you can do with maps.

## Binaries

A binary is group of consecutive bytes. You create them by surrounding the byte sequence with << and >>. Here we are creating a two-byte binary:

```
iex> <<5, 10>>
<<5, 10>>
```

In the decimal base, a byte can only contain values up to 255 (otherwise it overflows). If we want to store values greater than 255, we need to tell the runtime to use more space to store this binary:

```
iex> <<5, 256>>
<<5, 0>>
iex> <<5, 256::16>>
<<5, 1, 0>>
```

As you can see, when we specify the size (16 bits in this case) we can see that the output as an extra byte and the overflow didn't occur. The size doesn't have to be a multiple of 8. In that case, a binary is usually called a bitstring.

Most programmers will not handle data at such a low level, so your use of binaries may not be that frequent. However, they're extremely useful in certain scenarios, such as processing the header of a file to find a magic number and identify the file type, or even when dealing with network packets by hand.

## Strings and charlists

Strings are binaries with UTF-8 codepoints in them. You create a string with the usual double-quote syntax:

```
iex> "hey, a string"
"hey, a string"
```

Charlists are, as the name implies, lists of character codes. You create them using the single-quote syntax:

```
iex> 'hey, a charlist'
'hey, a charlist'
```

Since this is just a list, you can use the hd function to get the code for the first character:

```
iex> hd('hey, a charlist')
104
```

![](img/c636f4cfb07f25f1db535995e50706ed_30_0.png)

You can find out the code of a certain character with the ? operator. For instance, to find out the character code of a lowercase d, you'd use ?d.

Both representations support string interpolation:

```
iex> "two plus two is: #{2+2}"
"two plus two is: 4"
iex> 'four minus one is: #{4-1}'
'four minus one is: 3'
```

Both representations also support the heredoc notation, which is most commonly used to write documentation. To create it, use three single or double quotes:

```
iex> """
...> a string with heredoc notation
...> """
"a string with heredoc notation\n"
iex> '''
...> a charlist with heredoc notation
...> '''
'a charlist with heredoc notation\n'
```

![](img/c636f4cfb07f25f1db535995e50706ed_30_1.png)

The closing delimiter of a heredoc string/charlist must be on its own line.

Elixir provides sigils as another syntax to declare strings or charlists, which can be handy if you want to include quotes inside your string. You can use ~s to create a string and ~c to create a charlist (their uppercase versions, ~S and ~C, are similar but don't interpolate or escape characters):

```
iex> ~s(a string created by a sigil)
"a string created by a sigil"
```

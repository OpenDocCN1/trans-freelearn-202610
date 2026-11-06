This will only work for keys that already exist in the map—this constraint allows Elixir to optimize and reuse the fields list when updating a map. If you want to insert a new key, use the put function from the Map module:

```
iex> Map.put(map, :gender, "Male")
%{age: 1, gender: "Male", name: "Gabriel"}
```

As with all other types, in the official documentation, at https://hexdocs.pm/elixir/Map.html, you can find a pretty detailed reference on what you can do with maps.

# Binaries

A binary is group of consecutive bytes. You create them by surrounding the byte sequence with << and >>. Here we are creating a two-byte binary:

```
iex> <<5, 10>>
<<5, 10>>
```

In the decimal base, a byte can only contain values up to 255 (otherwise it overflows). If we want to store values greater that 255, we need to tell the runtime to use more space to store this binary:

```
iex> <<5, 256>>
<<5, 0>>
iex> <<5, 256::16>>
<<5, 1, 0>>
```

As you can see, when we specify the size (16 bits in this case) we can see that the output as an extra byte and the overflow didn't occur. The size doesn't have to be a multiple of 8. In that case, a binary is usually called a bitstring.

Most programmers will not handle data at such a low level, so your use of binaries may not be that frequent. However, they're extremely useful in certain scenarios, such as processing the header of a file to find a magic number and identify the file type, or even when dealing with network packets by hand.

# Strings and charlists

Strings are binaries with UTF-8 codepoints in them. You create a string with the usual double-quote syntax:

```
iex> "hey, a string"
"hey, a string"
```
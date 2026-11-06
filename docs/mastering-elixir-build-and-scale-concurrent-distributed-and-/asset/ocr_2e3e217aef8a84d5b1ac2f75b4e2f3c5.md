```
iex> set = MapSet.put(set, 2)
#MapSet<[1, 2]>
iex> set = MapSet.put(set, 1)
#MapSet<[1, 2]>
```

Sets, by definition, can't contain duplicates. So, inserting a value that's already there has no effect. You can find the documentation for the MapSet module at https://hexdocs.pm/elixir/MapSet.html.

There are three types, related to the underlying Erlang VM, that we have to mention before closing this section. They are as following:

+   - **Reference:** A reference is a type created by the Kernel.make_ref function. This functions creates an almost-unique reference, which gets repeated around every 2^82 calls. We will not use references in this book.
+   - **Port:** A port is a reference to a resource. The Erlang VM uses it to interact with external resources, such as an operating system process. We will talk a bit more about ports later in this chapter, when we discuss the interoperability between Elixir and Erlang.
+   - **PID:** A PID is the type used to identify processes in the Erlang VM. You'll see PIDs in action later in this book, when we start working with Erlang VM processes.

To complete this section, there's a function that we want to highlight: the i function, which is auto-imported from the Kernel module. You can use it to find out more information about a data type. It will print information about the data type of the term you pass as an argument. Here is an example with a string:

```
iex> i("a string")
Term
"a string"
Data type
  BitString
Byte size
  8
Description
  This is a string: a UTF-8 encoded binary. It's printed surrounded by
  "double quotes" because all UTF-8 encoded codepoints in it are printable.
Raw representation
  <<97, 32, 115, 116, 114, 105, 110, 103>>
Reference modules
  String, :binary
Implemented protocols
  IEx.Info, Collectable, List.Chars, String.Chars, Inspect
```
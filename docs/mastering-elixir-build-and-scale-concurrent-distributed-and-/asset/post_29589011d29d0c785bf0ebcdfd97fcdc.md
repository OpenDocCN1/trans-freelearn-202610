
```
[%File.Stat{access: :read_write, atime: {{2017, 12, 30}, {20, 8, 29}}, ctime: {{2017, 12, 30}, {20, 8, 25}}, gid: 100, inode: 3278529, links: 1, major_device: 65024, minor_device: 0, mode: 33188, mtime: {{2017, 12, 30}, {20, 8, 25}}, size: 270, type: :regular, uid: 1000}, %File.Stat{access: :read_write, atime: {{2017, 12, 31}, {16, 58, 56}}, ctime: {{2017, 12, 30}, {3, 40, 29}}, gid: 100, inode: 3290229, links: 1, major_device: 65024, minor_device: 0, mode: 33188, mtime: {{2017, 12, 30}, {3, 40, 29}}, size: 509, type: :regular, uid: 1000}]
```

As you can see, we now have two files in our %Folder{} struct.

![](img/c636f4cfb07f25f1db535995e50706ed_69_0.png)

Although structs are implemented on top of maps, they do not share protocol implementations with the Map module. This means that you can't, out of the box, iterate on a struct, as it doesn't implement the Enumerable protocol.

We'll end our little tour of structs with two more bits of information. First, if you don't provide a default value when defining the fields of a struct, nil will be assumed as its default value. Second, you can enforce that certain fields are required when creating your struct. You do that with the @enforce_keys module attribute. If we wanted to make sure path was provided when creating our %Folder{} struct, we would define it as following:

```
$ cat examples/folder_with_enforce_keys.ex
defmodule Folder do
  @enforce_keys :path

  defstruct name: "new folder", files_info: [], path: nil
end
```

If you don't provide path when creating this struct, ArgumentError will be raised:

```
iex> %Folder{}
** (ArgumentError) the following keys must also be given when building struct Folder: [:path]
  expanding struct: Folder.__struct__/1
    iex:46: (file)
iex> %Folder{path: "/a/b/c/"}
%Folder{files_info: [], name: "new folder", path: "/a/b/c/"}
```

## Bringing structs and protocols together

Now that we have the %Folder{} struct defined, we can define its implementation for the Size protocol.

We'll first define the implementation for the %File.Stat{} struct, as we can then use this to implement the protocol for %Folder{}. Here's the implementation for %File.Stat{}:

```
$ cat examples/size_implementations_file_stat_and_folder.ex
defimpl Size, for: File.Stat do
  def size(file_stat), do: file_stat.size
end

# ...
```

With this in place, our implementation for our %Folder{} struct is as follows:

```
$ cat examples/size_implementations_file_stat_and_folder.ex
# ...

defimpl Size, for: Folder do
  def size(folder) do
    folder.files_info
    |> Enum.map(&Size.size(&1))
    |> Enum.sum()
  end
end
```

To find out the size of a folder, we sum the size of each file it contains. As such, this implementation iterates through our files_info list, using the Size implementation for %File.Stat{} to get the size of each file, summing all the sizes in the end. In the following snippet, we can see this implementation being used on the folder variable we just defined:

```
iex> Size.size(folder)
779
```

With this, we can see the full power of mixing structs and protocols, which lets us have polymorphic functions based on the data type of their arguments. We now have a common interface, Size.size(data), that allows us to find out the size of pretty much anything we want, provided that we implement the Size protocol for the data type we're interested in.

## Tooling and ecosystems

The last section of this chapter is dedicated to one of the most renowned characteristics of Elixir: its incredible tooling. Also, since we can directly use Erlang libraries, we can take advantage of a very mature ecosystem that has been around for decades. First, let's begin with Elixir's interactive shell, IEx.

## IEx

In the beginning of this chapter, we provided some instructions on how to start and stop an Elixir shell. You've seen it in use throughout this chapter. We'll now show you some other interesting things you can do inside IEx.

In this chapter, we've been giving you links to the official documentation of several Elixir modules. You can access this documentation right from IEx, using the h command. Let's say you want to know more about the Enum module:

```
iex> h Enum
Enum
#...,
```

The preceding code provides a set of algorithms that enumerate over enumerables according to the Enumerable protocol.

Note that, for brevity, the output of this command was cropped.

You can even pass a fully qualified function name, and get information about it. Let's say, for example, you don't remember the order of the arguments for the Enum.map/2 function:

```
iex> h Enum.map/2
def map(enumerable, fun)
@spec map(t(), (element() -> any())) :: list()

Returns a list where each item is the result of invoking fun on each corresponding item of enumerable.
For maps, the function expects a key-value tuple.
## Examples

iex> Enum.map([1, 2, 3], fn(x) -> x * 2 end)
[2, 4, 6]

iex> Enum.map([a: 1, b: 2], fn({k, v}) -> {k, -v} end)
[a: -1, b: -2]
```

Another very interesting feature, which ships from Elixir 1.5 onwards, is the ability to set breakpoints from IEx. To showcase this, let's create a breakpoint on the StringHelper.palindrome?/1 function we've defined in the Functions and modules section, present in the "string_helper.ex" file:

```
iex> break! StringHelper.palindrome?/1
1
iex> StringHelper.palindrome?("abba")
Break reached: StringHelper.palindrome?/1 (string_helper.ex:2)
```

```
1: defmodule StringHelper do
2:   def palindrome?(term) when is_bitstring(term) do
3:     String.reverse(term) == term
4:   end
```

```
pry(1)> term
"abba"
pry(2)> whereami
Location: string_helper.ex:2
```

```
1: defmodule StringHelper do
2:   def palindrome?(term) when is_bitstring(term) do
3:     String.reverse(term) == term
4:   end
```

As you can see, you could access the argument passed to the function, `term`, and also use `whereami` to show the location where the breakpoint stopped the execution. To resume the execution, you use the `continue` command to go to the next breakpoint, or `respawn` to exit and start a new shell.

From Elixir 1.6 onward, you can also use pattern matching and guard clauses when setting a breakpoint. The breakpoint we defined earlier could also be set as `break! StringHelper.palindrome?("abba")`, which would make our breakpoint work when that function is called with "abba" as the argument.

![](img/c636f4cfb07f25f1db535995e50706ed_72_0.png)

You can also use `IEx.pry` to set a breakpoint on the source-code file, instead of doing it via IEx. Use the method that's most appealing to you.

To finish this section, we'll show you a handy feature of IEx: the ability to access values from the history. Sometimes, you call a certain function and realize afterward that you wanted to bind the result of that function call to a variable. If this happens, you can use the `v/1` function. You pass it a number, which represents the position of the expression from which you want to retrieve the value (starting at 1). You can also pass a negative number, which makes the position relative to the current expression in the shell. You can call this function without providing an argument, which is the same as calling `v(-1)`—which means we're getting the value from the last expression. Let's see an example:

```
iex> 7 * 3
21
iex> a = v
21
iex> a * 2
42
```

```
iex> v(-3)
21
```

There are more things you can do with IEx, such as configuring it or connecting to remote shells. Please refer to the official documentation (at https://hexdocs.pm/iex/IEx.html) for further usage examples of IEx.

## Mix

Mix is the Swiss-army knife of the Elixir tools. It's used to compile and run your application, manage your dependencies, run tests, and even for profiling your code. You can see which Mix tasks are available by typing `mix help` in your terminal. We won't explain its usage here, since the next chapter's purpose is to show you how you can use Mix to create and maintain an Elixir project.

## ExUnit

Elixir comes with a fully fledged unit test framework: ExUnit. We'll use it to write tests for the application built in the course of this book. Chapter 9, Finding Zen Through Testing, is dedicated to testing, where we'll demonstrate how you can write tests for your application that let you sleep at night.

## Erlang interoperability

As we stated at the beginning of this chapter, Elixir targets the Erlang runtime. Elixir is compiled to byte-code that can run on an Erlang VM (or BEAM), and Erlang libraries can be used in your Elixir projects (and vice versa). The philosophy in the Elixir community is to not reinvent the wheel and directly use Erlang libraries when appropriate. The creation of Elixir libraries that simply wrap an underlying Erlang library is discouraged, as you can directly call an Erlang library from your Elixir code.

We can take advantage not only of Erlang libraries, but also of their tooling. For instance, Erlang ships with a tool called Observer, which allows you to monitor your server and provides you with tons of useful information about the Erlang VM, such as the running processes or dynamic charts of load. We'll explore this tool in greater detail in Chapter 11, Keeping an Eye on Your Processes, when we talk about monitoring.

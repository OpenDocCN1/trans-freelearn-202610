Having to implement a protocol for all types may quickly become monotonous and exhausting. You can define a fallback behavior for types that don't implement your protocol by implementing the protocol for Any. Let's do this for our Size protocol:

```
$ cat examples/size_implementation_any.ex
defimpl Size, for: Any do
  def size(_), do: 0
end
```

You have to define the desired behavior when a type doesn't implement your protocol. In this case, we're saying that it has a size of 0 (which might not make sense, since the data type may have a size different than 0, but let's ignore that detail).

We now have two options for this implementation to be used: Either mark the modules where we want this fallback behavior with @derive [Size] (the List module, for instance), or use @fallback_to_any true in the definition of our Size protocol. The former is more laborious as you have to annotate each module that you want to assume the behavior for Any, while the latter is simpler since you make it work on all data types just by changing the definition of your protocol. In the Elixir community, explicitness is usually preferred, and, as such, you're more likely to see the @derive approach in Elixir projects.

While implementing protocols for Elixir's data types already opens a world of possibilities, we can only fully utilize Elixir's extensibility when we mix them with structs. We haven't yet talked about structs, so we'll introduce them in the next section.

# Structs

Structs are an abstraction built on top of maps. We define a struct inside a module, with the defstruct construct. The struct's name is the name of the module it's being defined in (which means you can only define one struct per module). To defstruct, we pass a keyword list, which contains the key-value pairs that define the fields that struct has, along with their default values. Let's define a Folder struct:

```
$ cat examples/folder.ex
defmodule Folder do
  defstruct name: "new folder", files_info: [], path: nil
end
```

We can now use it in our IEx session:

```
iex> %Folder{}
%Folder{files_info: [], name: "new folder", path: nil}
iex> %Folder{}.name
```
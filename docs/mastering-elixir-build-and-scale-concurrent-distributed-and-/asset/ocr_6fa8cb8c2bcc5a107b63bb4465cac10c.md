If we didn't implement all of the functions declared in the Presenter behaviour (by commenting the CLI Presenter.present/1 function, for instance), the Elixir compiler would emit the following warning:

```
warning: undefined behaviour function present/1 (for behaviour Presenter)
```

Behaviours help us follow the open/closed principle, since we can extend our system without modifying what we already have. If, in the future, we would need a new type of presenter, we'd just have to create a new module that adopts the Presenter behaviour, without having to change the behaviour or its current implementations. We'll be using behaviours in the application we'll build in this book, and you'll see them in action again in the next chapter.

# Protocols

Throughout this introductory chapter, we've mentioned a couple of times that Elixir has protocols, with Enumerable being one of the examples. In this section, we'll dive into protocols and even define our own!

Protocols, like the behaviours we've seen in the last section, define a set of functions that have to be implemented. In that sense, both constructs serve as a way to achieve polymorphism in Elixir–being able to display multiple forms of behavior, but all linked to a single interface. While behaviours define a set of functions that a module needs to implement, and are thus tied to a module, protocols define a set of functions that a data type must implement. This means that, with protocols, we have data type polymorphism, and we're able to write functions that behave differently depending on the type of their arguments.

Let's now see how we can create a new protocol. We'll pick up, and extend, the example present in the official Getting Started guide (at http://elixir-lang.github.io/getting-started/protocols.html). We will define a Size protocol, which will be implemented by each data type. To define a new protocol, we use the defprotocol construct:

```
$ cat examples/size.ex
defprotocol Size do
  @doc "Calculates the size of a data structure"
  def size(data)
end
```

We're stating that our Size protocol expects the data types that will implement it must define a size/1 function, where the argument is the data structure we want to know the size of.
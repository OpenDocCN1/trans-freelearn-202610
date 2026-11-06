# Elixir and Erlang

Elixir, created by José Valim, runs on the Erlang VM (also known as BEAM). Erlang, developed at Ericsson more than 30 years ago, was created to improve the reliability and scalability of the company's telecom systems. Nowadays, it is used in a number of different settings, from database systems to chat applications. Erlang has fault-tolerance and distribution baked into its design, and is famous for running systems with nine nines of reliability.

Erlang's runtime is natively distributed, given that it was designed to be deployed on multiple telecom switches simultaneously. Programs running on the Erlang VM can take advantage of this by easily distributing an application across multiple nodes, but also across multiple CPUs—since multiple cores is just a specific case of a distributed system. This is an incredible selling point of the Erlang VM (and thus of Elixir), since in today's setting CPUs are not getting much faster, and we're instead seeing CPUs with an increasing number of cores coming out.

In this opening chapter, we will be introducing Elixir. Beginning with its data types, we will also look at pattern matching, anonymous and named functions, modules, and some control-flow constructs. Then, we will see how to work with collections, and then we will briefly touch on behaviours and protocols. The chapter will end with an overview of the incredible tooling Elixir provides, along with some ways to exploit the existing interoperability between Elixir and Erlang.

# Elixir's data types

We will now describe Elixir's data types, which extend upon Erlang's data types. Elixir is a dynamic programming language. Consequently, you don't declare the type of each variable—it depends on the value it holds at each moment.

To improve the learning experience, we'll be providing some examples along the way. For now, we'll just use Elixir's REPL, IEx (short for Interactive Elixir). To start an IEx session, you must have Elixir installed on your machine. Elixir has an official page with instructions on how to do this if you don't have it installed, whether using package managers, the precompiled version, or compiling from the source yourself:

```
http://elixir-lang.github.io/install.html
```
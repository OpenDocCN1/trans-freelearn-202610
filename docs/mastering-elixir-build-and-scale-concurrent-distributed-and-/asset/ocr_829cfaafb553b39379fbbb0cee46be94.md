# Using a remote shell

If the information provided by the observer isn't enough, with this setup you can also run a remote shell from the comfort of your local shell:

```
$ iex --name console@10.40.0.41 --cookie $COOKIE --remsh
"elixir_drip@10.40.0.41"
Erlang/OTP 20 [erts-9.3] [source] [64-bit] [smp:4:4] [ds:4:4:10] [async-threads:10] [hipe] [kernel-poll:false]

Interactive Elixir (1.6.4) - press Ctrl+C to exit (type h() ENTER for help)
iex(elixir_drip@10.40.0.41)> node()
:"elixir_drip@10.40.0.41"
iex(elixir_drip@10.40.0.41)> Node.list
[:"elixir_drip@10.40.2.143", :"elixir_drip@10.40.0.42",
:"console@10.40.0.41"]
```

As you can see, the current node (given by the Kernel.node/0 function) is the remote one (elixir_drip@10.40.0.41), so whatever you run will be run on the remote node. Because we have the ElixirDrip nodes connected with the libcluster library, we see three nodes returned by the Node.list/0 function.

Let's put the remote shell and observer tools to work for us by storing a file through remote shell and then looking at its contents with observer:

```
iex(elixir_drip@10.40.0.41)> jose_user =
ElixirDrip.Accounts.get_user_by_username("jose")
%ElixirDrip.Accounts.User{
  # ...
  username: "jose"
}

iex(elixir_drip@10.40.0.41)> ElixirDrip.Storage.store(jose_user.id,
"remote_shell_test.txt", "$/foo/bar", "f○e? V○eV ... ( ^_ ) ( ^_^ ) Hello from Remote Shell")
{:ok, :upload_enqueued,
%ElixirDrip.Storage.Media{
  file_name: "remote_shell_test.txt",
  file_size: 95,
  full_path: "$/foo/bar",
  id: "173aBUSCM8nVJKPUDB e6EondStn",
  # ...
}}
```
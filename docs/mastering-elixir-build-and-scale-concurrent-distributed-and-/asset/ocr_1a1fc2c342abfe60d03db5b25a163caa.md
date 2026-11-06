This way, _ will match messages that didn't match on the clauses before it, having a response for this message and removing it from the mailbox. Then, we return an error tuple, which could also include reporting (either to logs or an error-tracking system), so that this can be analyzed later and also possibly track down the source of the unexpected messages.

![](bbox=[0.12, 0.24, 0.2, 0.33])

When developing and/or debugging in an IEx session, it may be helpful to use the `IEx.Helpers.flush/0` function, which will remove all the messages from the mailbox of the shell process and print them out to the terminal.

As we've seen, `receive` will fetch a message from the mailbox of the process, and try to match it to the provided clauses. So far, we've called `receive` only after sending the messages to the shell process. If we call it with an empty mailbox, `receive` will block, waiting for a message:

```
```elixir
iex> receive do
...>   message -> IO.puts "Message is #{message}"
...> end
```
```

The shell is now blocked and you have to manually terminate it (by pressing `Ctrl-\`, for instance). We would observe the same behavior if there were messages in the mailbox that didn't match any of the provided clauses. If we don't want `receive` to wait indefinitely for messages, we can provide an `after` clause, specifying the time we want `receive` to wait, in milliseconds:

```
```elixir
iex> receive do
...> message -> IO.puts "Message is #{message}"
...> after
...> 2000 -> IO.puts "Timed out. No message received in 2 seconds."
...> end
Timed out. No message received in 2 seconds.
:ok
```
```

As you learned in the first chapter, everything in Elixir is an expression. Similar to what happens with the `case` construct, the return value of `receive` is the result of the last expression on the clause that was executed.

We'll end this section with a more elaborate example. Now that we know how to create processes, and also how to pass messages between them, let's tie all of this together, while also showcasing some advantages of having processes running concurrently.
# Preparing the server

We'll now adapt our server to allow clients to connect using Phoenix Channels. Just as with traditional web applications, our journey begins in the endpoint. When Phoenix generated the endpoint module, it included the following line:

```
$ cat apps/elixir_drip_web/lib/elixir_drip_web/endpoint.ex

defmodule ElixirDripWeb.Endpoint do
  # ...

  socket("/socket", ElixirDripWeb.UserSocket)

  # ...
end
```

Phoenix.Endpoint.socket/2 defines a mount point for a socket handler, which will contain channel definitions. In this case, we're saying that clients that want to connect to a channel may connect to the /socket path, which will be handled by the module we're passing as the second argument. Let's now see the code for that module:

```
$ cat apps/elixir_drip_web/lib/elixir_drip_web/channels/user_socket.ex

defmodule ElixirDripWeb.UserSocket do
  use Phoenix.Socket

  ## Channels
  channel "users:*", ElixirDripWeb.UserChannel

  ## Transports
  transport(:websocket, Phoenix.Transports.WebSocket)
  # transport :longpoll, Phoenix.Transports.LongPoll

  def connect(_params, socket) do
    {:ok, socket}
  end

  def id(_socket), do: nil
end
```
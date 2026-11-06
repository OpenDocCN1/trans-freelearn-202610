```
```elixir
# ...

def init(format), do: format

def call(conn, format) do
  case conn.assigns[:current_user] do
    nil -> logged_out(conn, format)
    _ -> conn
  end
end

# ...

defp logged_out(conn, :html) do
  conn
  |> put_flash(:error, "You need to be logged in to view this content.")
  |> halt()
  |> redirect(to: page_path(conn, :index))
end

defp logged_out(conn, :json) do
  conn
  |> put_status(:unauthorized)
  |> halt()
  |> render(ElixirDripWeb.ErrorView, "401.json", message:
    "Unauthenticated user.")
end
end
```
```

The logic to handle logged in users hasn't changed. If there's a current user in conn.assigns, we simply return the connection, unmodified. It's when we need to handle logged out users that the format of the request is necessary. As we've seen in the beginning of this chapter, the options passed to module plugs are then served as the second argument of the call/2 function. Now, we make use of that argument and pass it to the logged_out/2 function. We use pattern-matching to handle each request format accordingly. Handling logged out users for HTML requests is something we already had in this module; we've merely extracted it to this function. The new part is the handling of logged out users for JSON requests. In this case, we're putting the correct status code for the response, halting the Plug pipeline, and then rendering the error view, passing "Unauthenticated user." as the message assign.
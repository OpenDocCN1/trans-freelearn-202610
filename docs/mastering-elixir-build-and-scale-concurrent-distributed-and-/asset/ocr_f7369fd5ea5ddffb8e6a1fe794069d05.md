```
setup [:create_user, :login_user, :create_media]

test "GET /files lists the files for the current user", %{conn: conn, media: media} do
  response = get(conn, "/files?path=$")

  assert html_response(response, 200) =~ "test.txt"
  assert html_response(response, 200) =~ "/files/#
{media.id}/download"
end

defp create_user(_context) do
  {:ok, user} =
    ElixirDrip.Accounts.create_user(%{
      username: "test",
      password: "12345678",
      email: "test@test.com"
    })

  {:ok, user: user}
end

defp login_user(%{conn: conn, user: user}) do
  conn = Plug.Test.init_test_session(conn, user_id: user.id)
  {:ok, conn: conn}
end

defp create_media(%{user: user}) do
  {:ok, :upload_enqueued, media} =
    ElixirDrip.Storage.store(user.id, "test.txt", "$", "some cool content")

  {:ok, media: media}
end
end
```

We begin by calling use ElixirDripWeb.ConnCase, which will import some convenience modules to create and use connections in tests. If you want to check its contents, it's available at apps/elixir_drip_web/test/support/conn_case.ex. We then have a setup with three functions, which will create and log in a user, and then create a media file for this user. The creation of the user and its media work the same way as in the StorageRepoTest we've seen earlier. The new part here is the login_user function. This setup function uses the Plug.Test.init_test_session/2 function to initialize a session, essentially logging in the previously created user. This way, our test requests will run just as a real one would, including going through the authentication Plugs we created in the last chapter.
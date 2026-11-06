By passing this option, we can use this path helper as api_file_path(conn, :index), with the one in the application scope being called in the same way.

Looking now at the next scope, we can see that the path prefix is "api". It's in this scope that we'll have all the routes related to our API, and each one will call the :api pipeline we described before. Note that we're passing a different module name prefix in this scope, ElixirDripWeb.Api. This way, we'll have a different controller to handle API requests. There are different ways to implement this. We could, for instance, have the same controller handle both types of request, and then use the Phoenix.Controller.get_format/1 function to know whether the incoming request is HTML or JSON.

We've opted to have separate controllers because we have code very specific to the HTML requests in our file controller, such as the put_flash function. We feel that the code is easier to read and comprehend if we have the logic for each type of request in its own module. Let's now look at the file controller for the API request:

```
$ cat apps/elixir_drip_web/lib/elixir_drip_web/api/controllers/file_controller.ex

defmodule ElixirDripWeb.Api.FileController do
  use ElixirDripWeb, :controller

  alias ElixirDrip.Storage

  def index(conn, %{"path" => path}) do
    with {:ok, media} <- Storage.media_by_folder("hardcoded_user_id", path) do
      render(conn, files: media.files, folders: media.folders)
    else
      {:error, :invalid_path} ->
        render(conn, ElixirDripWeb.ErrorView, "400.json", message: "The path provided is invalid.")
    end
  end

  def index(conn, _params), do: render(conn, ElixirDripWeb.ErrorView, "400.json", message: "Missing 'path' query parameter")
end
```

For now, we don't have authentication via the API, so we're just passing a hardcoded user ID to the media_by_folder/2 function. This code is very similar to the one for the index action in the HTML controller. When media_by_folder/2 returns the expected result, we call the render function, passing the files and folders assigns as we did in the HTML controller.
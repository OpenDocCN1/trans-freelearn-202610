Before closing this section and moving on to authenticating socket connections, let's see how we can send events from the client side. To demonstrate this, we'll create the functionality that allows users to share a file with other users. This will be entirely driven by channels, without any change on the router or the controllers.

We've added a button to the show action of a file, and defined the callback of that button to be the following function:

```
$ cat apps/elixir_drip_web/assets/js/notification.js

let Notification = {
  // ...
  sharePopUp(userChannel) {
    let username = prompt("Share file with:", "username")
    if (username != null && username != "") {
      let splitUrl = window.location.href.split('/')
      let fileId = splitUrl.pop() || splitUrl.pop()

      userChannel.push("share", {username: username, file_id: fileId})
    }
  }
}

export default Notification
```

This function receives a userChannel argument, which is the channel we created previously on this file. We begin by rendering a prompt, so that the user can write the username of the user we're going to share this file with. Then, we have a couple of lines to extract the file ID from the current path. This is just preparation for the function call that matters: in the end, we push a share message to the userChannel. We provide the name of the event, along with the necessary parameters for the share to occur. This will emit the event on the current user's channel. Let's now see how to handle this event on the server. We need to adapt the user channel module:

```
$ cat apps/elixir_drip_web/lib/elixir_drip_web/channels/user_channel.ex

defmodule ElixirDripWeb.UserChannel do
  # ...
  alias ElixirDripWeb.Endpoint
  alias ElixirDrip.{Accounts, Storage}

  def handle_in("share", %{"username" => username, "file_id" =>
  file_id}, socket) do
    sharee = Accounts.get_user_by_username(username)
    case sharee do
      nil -> broadcast_sharee_not_found(socket, username)
```
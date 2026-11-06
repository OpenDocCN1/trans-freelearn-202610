```
    payload: %{message: "test has just shared a file with you!"}
    }
    end

    # ...
    end
```

Before diving into the tests, let's first recap the channels we have in our application, as it will make the test easier to follow. First, we have the "users:lobby" topic, which all users connect to. This is used to track who's online at the current moment, and is powered by Phoenix.Presence. We then have a different topic for each user. The topic is of the format "users:<user_id>", where <user_id> represents the ID of the user that's connecting to the socket. This channel is used to deliver notifications only to a single user, such as when a download or upload has been completed.

To unclutter the explanation, we're hiding the setup functions, as they're similar to what we've already seen. We're starting to see a pattern, and we could already be benefiting from having our own Case, where users and media are created. However, note that in this case the requirements are a bit different. We now need to create two users: one named test, which is the owner of the created media, and another user named other_user, which is the user we'll share the media with. If we had prematurely abstracted the creation of users to a Case, we'd now need to stop and try to adapt it to this new requirement. Our advice on this is to delay as much as possible this type of extraction, and first establish the bulk of the tests your application will have. Once you have them, the patterns will naturally emerge and you'll be able to see where you can remove duplication, without hindering the readability of the tests. As for the connect_to_socket function, it's just using the connect function to establish a connection, just like we did in the UserSocket tests.

In our first test, we're checking that joining the "users:lobby" channel works as expected. In this case, we want to validate that a "presence_state" event is pushed back to the user who just joined, as this is the only event emitted when joining this channel. We're using the assert_push helper, which will ensure that the channel has pushed a message for the given event back to the user.

The next test is a bit more complex. We now want to test that when a "share" event is received in the server, it will call the necessary functions to share the file with another user, but will also emit the events to the owner of the file and the sharee, informing that the sharing was successful. To test this, we need to subscribe to two channels: the one for the owner of the file, using the subscribe_and_join function, as we did previously, and the one for the sharee user, which we do using the ElixirDripWeb.Endpoint.subscribe function.
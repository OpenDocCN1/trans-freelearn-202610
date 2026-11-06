```
iex> Logger.debug "#{inspect(self())} Start";
Search.safe_task_stream_search_for(["25", "8", "6", "7"], "media")

10:14:06.861 #PID<0.463.0> Start

10:14:06.873 Fetching cached content for 25...
10:14:06.873 Fetching cached content for 8...
10:14:06.873 Fetching cached content for 6...
10:14:06.874 Fetching cached content for 7...

# ...
Seconds elapsed: 6 secs
10:14:12.874 #PID<0.473.0>: Received :get_media and served
"Content of media 6." bytes 1 times.

10:14:13.868 Seconds elapsed: 7 secs
10:14:13.874 #PID<0.474.0>: Received :get_media and served
"Content of media 7." bytes 1 times.

10:14:14.869 Seconds elapsed: 8 secs
10:14:14.874 #PID<0.475.0>: Received :get_media and served
"Content of media 8." bytes 1 times.

# ...
10:14:16.871 Seconds elapsed: 10 secs

%{
  "6" => [{1, "Content of media 6."}],
  "7" => [{1, "Content of media 7."}],
  "8" => [{1, "Content of media 8."}]
}
```

We can finally observe the results of our text search: for each media id, it returns a list with the number and the contents of the line that matched the search expression. It always returns in the time span we defined, so we can deliver snappier results to our users.

Let's now see how we can use the ETS tool that comes with Erlang/OTP to store the results of text searches performed on ElixirDrip.

# Using (D)ETS

In the previous section, we implemented the ElixirDrip text search functionality by using Task.async_stream/5 to search several text files at the same time. The text search function lived on the ElixirDrip.Storage.Search module and its last iteration could be used like this:
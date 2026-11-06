```
Map.update(accum, media.user_id, 1, &(&1 + 1))
end
```

Looking at the preceding query, one can easily discern its steps: we grab the media_set, create a new set of stages using the :user_id field to partition the items, and then we count the existing media files by user. When compared to the eager version of this query, the changes we made aren't that big and the actual reduce operation has the exact same semantics as the eager reduce operation. With minimal effort, we got a lazy parallel query that can leverage all the available resources of the machine. This is the delight of every programmer who wants to maximize the efficiency of their code.

# Disk-usage rank query

In the previous section, we ended up getting the number of files per user. We will now iterate on the previous result to create a disk-usage ranking. Since we want the disk usage, we will have to update the previous files_by_user_v3 flow so that it accumulates each file size per user id as well:

```
$ cat examples/search.exs
import_file("./examples/sample_data.exs")
# ...
disk_usage_by_user = media_set
|> Flow.from_enumerable(max_demand: 1)
|> Flow.partition(key: {:key, :user_id})
|> Flow.reduce(fn -> %{} end,
fn %{user_id: user_id, file_size: size}, accum ->
Map.update(accum, user_id, size, &(&1 + size))
end)
```

The previous disk_usage_by_user flow gives us the expected results, showing each user's disk usage. User 3 is clearly using much more space than the other users:

```
# 'search.exs' already evaluated

iex> disk_usage_by_user |> Enum.to_list()
[{2, 540}, {1, 8695}, {3, 21868}, {4, 5922}]
```
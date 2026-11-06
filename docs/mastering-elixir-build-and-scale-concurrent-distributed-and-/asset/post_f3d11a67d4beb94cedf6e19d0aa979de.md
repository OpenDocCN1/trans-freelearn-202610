
```
Map.update(accum, media.user_id, 1, &(&1 + 1))
end
```

On the preceding function, we are passing a lambda function as the :hash option to the Flow.partition/2 call. This lambda receives an element from the collection and has to yield a two-element tuple, which contains the resulting element that will be emitted and the value of the hashing function that will point to the right stage.

![](img/c636f4cfb07f25f1db535995e50706ed_224_0.png)

We are subtracting one from the user_id value: each stage created by the partition/2 call is 0-index based, hence for N users starting with id=1, we want N-1 hashing values starting at 0. If we incorrectly defined the :hash lambda as fn m -> {m, m.user_id} end, we would obtain something like [{2, 1}, {1, 1}, {3, 3}]. As you can see, the single file user 4 had wasn't considered, because it was allocated to stage 4, and we only had stages 0 to 3.

With this :hash function, we now get the expected result. You should also be aware that with Flow, results aren't sorted by default. We don't control how fast results are produced, so no order is guaranteed. We can now see that user 3 has three files on ElixirDrip, and the other three users only have one each:

```
# 'search.exs' already evaluated

iex> files_by_user_v2 |> Enum.to_list()
[{1, 1}, {2, 1}, {4, 1}, {3, 3}]
```

In situations where we want to distribute the collection items by stages and we have each item uniquely identified by a key, we could instead use the :key option of the Flow.partition/2 function. This option allows you to not implement the hashing function at all. If you pass the {:key, :user_id} tuple as this option value, it assumes all items are maps in order to use the :user_id field of the item as the hashing value. If you pass {:elem, 2} instead, it assumes all items are tuples and it should use the third element (tuples are 0-index based) as the hashing value to route items to stages.

Let's iterate just one more time on the preceding version we already had working:

```
$ cat examples/search.exs
import_file("./examples/sample_data.exs")
# ...
files_by_user_v3 = media_set
|> Flow.from_enumerable(max_demand: 1)
|> Flow.partition(key: {:key, :user_id})
|> Flow.reduce(fn -> %{} end,
fn media, accum ->
Map.update(accum, media.user_id, 1, &(&1 + 1))
end
```

Looking at the preceding query, one can easily discern its steps: we grab the media_set, create a new set of stages using the :user_id field to partition the items, and then we count the existing media files by user. When compared to the eager version of this query, the changes we made aren't that big and the actual reduce operation has the exact same semantics as the eager reduce operation. With minimal effort, we got a lazy parallel query that can leverage all the available resources of the machine. This is the delight of every programmer who wants to maximize the efficiency of their code.

## Disk-usage rank query

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

But who is this user 3? It would be nice to know the names of our users along with their disk usage, instead of only their ids. Since we have a flow on both the disk_usage_by_user and pretty_users variables, we can combine them to create a new lazy and parallel query so we know who our users actually are. This combination will be performed by the Flow.bounded_join/6 function, which will receive the two flows and will combine them into a new, single flow, by matching the items of one flow to the items of the other.

Let's create our disk_usage_ranking_v1 flow, based on the previous two flows we had. To join the existing flows, we have to point out to the Flow.bounded_join/6 function how it should match the events flowing from each individual flow. The &(&1.id) lambda shows how the id is retrieved from each pretty_users item, whereas the &(elem(&1, 0)) indicates how the id should be fetched from the disk_usage_by_user flow. The last lambda receives a single element from each flow through its two arguments and lets you emit new events. In this case, we are emitting a new map with user and disk fields, where the value of the first comes from the pretty_users flow and the value of the latter comes from the disk_usage_by_user one:

```
$ cat examples/search.exs
import_file("./examples/sample_data.exs")
# ...
disk_usage_ranking_v1 = Flow.bounded_join(
  :inner,
  pretty_users,
  disk_usage_by_user,
  &(&1.id),
  &(elem(&1, 0)),
  fn user, {_user_id, total_size} ->
    %{user: user.full_name, disk_usage: total_size/1000}
  end)
|> Enum.sort(&(&1.disk_usage >= &2.disk_usage))
```

Since Flow doesn't ensure any order, in the end we sort the resulting list by descending order of disk_usage value. Let's now try our joint flow:

```
# 'search.exs' already evaluated

iex> disk_usage_ranking_v1 |> Enum.to_list()
[
  %{disk_usage: 21.868, user: "Jose Valim"},
  %{disk_usage: 8.695, user: "Andre Albuquerque"},
  %{disk_usage: 5.922, user: "Joe Armstrong"},
  %{disk_usage: 0.54, user: "Daniel Caixinha"}
]
```

We now get a much prettier result, and we can see how José is using almost 22 megabytes on ElixirDrip. However, we had nine users. Why are we seeing just a subset of users in our ranking? Similar to what `inner join` does on SQL, we are doing an `:inner` bounded join here (look at the first argument passed to the `Flow.bounded_join/6` function). This means the resulting flow will only have events if there were matching items on the left and right input flows. In our case, we were only emitting events for users on the `pretty_users` flow that also had files on the `disk_usage_by_user` flow.

What we really want is to have events on the resulting flow for all users, independently of each user having files or not. To achieve this, we simply need to change the bounded join mode from `:inner` to `:left`, since the `left` flow is the `pretty_users` flow. This way, it will emit new events for every item on the `left` flow, independently of having a corresponding item on the `right` flow or not:

```
$ cat examples/search.exs
import_file("./examples/sample_data.exs")
# ...
disk_usage_ranking_v2 = Flow.bounded_join(
  :left_outer,
  pretty_users,
  disk_usage_by_user,
  &(&1.id),
  &(&1.id),
  fn user, right_elem ->
    disk_usage = case right_elem do
      nil -> 0
      {_user_id, total_size} -> total_size
    end

    %{user: user.full_name, disk_usage: disk_usage}
  end)
|> Enum.sort(&(&1.disk_usage >= &2.disk_usage))
```

As you can see, we had to change the last lambda function, responsible for the emission of new events. Since we are now doing a `:left` bounded join, there will be times when a `user` has no entry on the `disk_usage_by_user` flow. When this happens, the function argument representing the element from the `right` flow is passed as `nil`, and our lambda function has to cater for it. In our case, if the `right_elem` parameter is `nil`, we know the user isn't using any disk space so we simply return 0 as the `disk_usage` value.

We now have a proper disk-usage ranking, showing each user's name:

```
# 'search.exs' already evaluated

iex> disk_usage_ranking_v2 |> Enum.to_list()
[
  %{disk_usage: 21868, user: "Jose Valim"},
  %{disk_usage: 8695, user: "Andre Albuquerque"},
  %{disk_usage: 5922, user: "Joe Armstrong"},
  %{disk_usage: 540, user: "Daniel Caixinha"},
  %{disk_usage: 0, user: "Mike Williams"},
  %{disk_usage: 0, user: "Atenas"},
  %{disk_usage: 0, user: "Robert Virding"},
  %{disk_usage: 0, user: "Jose Lusquinos"},
  %{disk_usage: 0, user: "Billy Boy"}
]
```

In all the previous examples, we used Flow with finite data sets. This means that after the work is done, the spawned processes are ended since they won't need to process any more events. However, one could also use Flow to process infinite streams of data. On the ElixirDrip sphere, imagine that we wanted a disk-usage ranking updated in realtime. In this case, we would have to decide when the ranking should be updated, since by definition we couldn't wait for every upload to be done to then update the ranking. This is where the concept of window comes in: Flow lets you create a pipeline that, for example, processes events every five minutes, buffering them in the meantime. There's much more to be said about processing infinite data with Flow, so do not consider it only as a possible tool to handle finite data sets.

As we observed throughout this section, when compared with GenStage, Flow allows us to work with events in a streamlined way. Since it sits on top of GenStage, by using Flow, one benefits from all the GenStage features but should also be aware of the associated process-spawning and message-passing overhead that comes with it. As any tool out there, Flow is not a silver bullet; it shines on contexts where the data to process spans gigabytes of data and/or the event-processing is CPU- or I/O-intensive, but ultimately one should rely on benchmarks to decide whether to use Flow or not.

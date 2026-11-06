```
] 

iex> raw_media_set |> Enum.count()
6
```

Let's filter the raw_media_set on our search.exs script so that we only keep the needed fields in the collection:

```
$ cat examples/search.exs
import_file("./examples/sample_data.exs")
# ...
media_set = raw_media_set
  |> Flow.from_enumerable()
  |> Flow.map(&Map.take(&1, [:id, :user_id, :file_name,
:file_size]))
  |> Enum.to_list()
```

By running the previous script, we now get a streamlined media_set list, with just the fields we need:

```
# 'search.exs' already evaluated

iex> media_set
[
%{file_name: "ylcab53cvp5gauei.doc", file_size: 8915, id: 1, user_id: 3},
%{file_name: "5clgulo2hg241ayj.mp3", file_size: 7644, id: 4, user_id: 3},
%{file_name: "ym5yj435shtlenih.png", file_size: 5309, id: 6, user_id: 3},
%{file_name: "qvs3igorje7gc4ko.mp3", file_size: 540, id: 2, user_id: 2},
%{file_name: "cgdsam3gqp56gzvk.mp3", file_size: 8695, id: 3, user_id: 1},
%{file_name: "m4yijgulci jddvmp.mp3", file_size: 5922, id: 5, user_id: 4}
]
```

With the media_set prepared, we're now ready to start "flowing" it into our data query pipelines.

# Lazy queries

The first thing we want is to use Flow to count how many files each user has currently stored on ElixirDrip. In practice, we want to reduce our media_set, by incrementing each user's counter for each file. The naive approach could be something like the following:

```
$ cat examples/search.exs
import_file("./examples/sample_data.exs")
# ...
files_by_user_v1 = media_set
  |> Flow.from_enumerable(max_demand: 1)
```
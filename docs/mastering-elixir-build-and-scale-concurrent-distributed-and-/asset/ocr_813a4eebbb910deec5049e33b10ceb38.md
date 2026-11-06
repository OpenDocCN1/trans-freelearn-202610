```
iex> alias ElixirDrip.Storage.Search

iex> media_ids = ["25", "8", "6", "7"]

iex> search_expression = "media"

iex> Search.safe_task_stream_search_for(media_ids,
search_expression)

%{
  "6" => [{1, "Content of media 6."}],
  "7" => [{1, "Content of media 7."}],
  "8" => [{1, "Content of media 8."}]
}
```

It returned a map with entries for every searched file whose search ended on the allotted time. For each media id, it yields a list of two-element tuples, where the first tuple element corresponds to the line where the expression was found and the second element contains the actual line's contents.

Remember that ElixirDrip spawns a CacheWorker to keep files in memory for a while to speed up consecutive downloads, but reaps those processes after a while in order to keep the memory usage in check. The Search.search_for/2 function that performs the actual file search assumes there is already a CacheWorker process for the file to be searched. Hence, if we want to search a specific text file, we will have to download it before actually searching through it.

The objective here is to implement a cache to store search results in order to improve subsequent searches. Instead of immediately downloading a file only to search through its contents, we will first take a look at the search results cache to find whether the same search had already been done.

We will implement this cache using the ETS, an Erlang OTP construct that allow us to efficiently store and query arbitrary Erlang terms in memory. We use the term efficiently here because an ETS table gives you constant access time, no matter the size of the data you store on it. Since ETS data lives in memory, it shouldn't strike as a surprise if we tell you that each ETS table belongs to a specific process. In a scenario where the ETS data should be kept, we have to cautiously handle the termination of the process holding the corresponding ETS table to ensure that the table doesn't get lost.

Let's create a module that's a GenServer based SearchCacheWorker module that will encapsulate the logic of our search results cache. Since we will have only one SearchCacheWorker, we are registering it under the ElixirDrip.Storage.Workers.SearchCacheWorker alias (its module name).
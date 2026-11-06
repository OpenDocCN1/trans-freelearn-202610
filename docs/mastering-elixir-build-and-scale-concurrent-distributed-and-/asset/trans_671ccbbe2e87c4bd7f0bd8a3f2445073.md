
由于表只能由其进程所有者写入，此函数将简单地向进程所有者执行一次 GenServer.call/2 调用，然后由其在对应的 handle_call/3 回调中负责更新 ETS 表：

```
$ cat apps/elixir_drip/lib/elixir_drip/storage/workers/
search_cache_worker.ex
defmodule ElixirDrip.Storage.Workers.SearchCacheWorker do
  # ...
  def cache_search_result(media_id, search_expression, result) do
    GenServer.call(__MODULE__, {:put, media_id, search_expression,
    result})
  end

  def handle_call({:put, media_id, search_expression, result}, _from,
  search_cache) do
    created_at = :os.system_time(:seconds)

    result = :ets.insert_new(search_cache, {{media_id,
    search_expression}, {created_at, result}})

    {:reply, {:ok, result}, search_cache}
  end
end
```

为了向 ETS 表中插入新值，我们将表名（存储在进程状态中）和新条目传递给 :ets.insert_new/2 函数。此新条目始终必须是一个以键为第一个元素的元组。在我们的例子中，我们将 {{media_id, search_expression}, {created_at, result}} 作为第二个参数传递，因此我们的键是 {media_id, search_expression} 元组，其值是另一个包含 created_at 时间戳和实际文本搜索结果的元组（在本节末尾，你将明白我们为什么也要存储时间戳）。:ets.insert_new/2 调用将返回一个布尔值，表示操作是否成功：

```
iex> SearchCacheWorker.cache_search_result("1", "media",
"lorem ipsum media 1 contents")
{:ok, true}

iex> SearchCacheWorker.cache_search_result("1", "media",
"this can't be cached")
{:ok, false}

iex> SearchCacheWorker.cache_search_result("1", "other",
"other phrase found on media 1 contents")
{:ok, true}
```

我们将表的访问模式设置为 :protected，因此每个进程都可以读取其内容。

在此场景中，我们无需要求所有者进程为我们获取表中存储的数据，可以直接与其交互。这就是为什么我们将表名存储在 @search_cache 模块属性上的原因：由于每个进程都可以从表中读取，表名不能仅存在于进程所有者的状态中。

要访问表条目，可以使用 :ets.lookup/2 函数，它接收表名或引用以及我们要查找的键。该函数返回具有该键的条目列表。在我们的例子中，每个返回的列表只有一个元素，因为我们使用的是 :set ETS 表：

```
iex> :ets.lookup(:search_cache, {"1", "media"})
[{"1", "media", {1518475143, "lorem ipsum media 1 contents"}}]

iex> :ets.lookup(:search_cache, {"1", "unknown"})
[]
```

正如你刚才看到的，当存在该键的条目时，我们会得到一个包含 {key, value} 元组的列表。为了更方便地获取存储的值，我们将 :ets.lookup/2 函数封装在了 search_result_for/2 函数中：

```
$ cat apps/elixir_drip/lib/elixir_drip/storage/workers/search_cache_worker.ex
defmodule ElixirDrip.Storage.Workers.SearchCacheWorker do
  # ...
  @search_cache :search_cache

  def search_result_for(media_id, search_expression) do
    case :ets.lookup(@search_cache, {media_id, search_expression}) do
      [] ->
        nil
      [{_key, {_created_at, search_result}}] ->
        search_result
    end
  end
end
```

使用之前的函数，我们无法获取给定 media_id 缓存的所有文本搜索。如果要实现它，我们需要查询只查看键元组的第一个元素，其中包含 media_id。:ets.match_object/2 函数正好可以做到这一点：使用它，你可以对特定字段进行模式匹配：

```
iex> :ets.match_object(:search_cache, {{"1", :"_"}, :"_"})
[
  {{"1", "other"}, {1518509888, "other phrase found on media 1 contents"}},
  {{"1", "media"}, {1518475143, "lorem ipsum media 1 contents"}}
]
```

让我们创建一个 all_search_results_for/1 函数，对键元组的 media_id 元素进行模式匹配，类似于我们刚才的做法。我们使用了特殊形式 : "_" 来表示，对于我们的 {{media_id, search_expression}, {created_at, result}} 条目，我们不关心 search_expression 元组元素，也不关心每个条目值对应的最后一个元组：

```
$ cat apps/elixir_drip/lib/elixir_drip/storage/workers/
search_cache_worker.ex
defmodule ElixirDrip.Storage.Workers.SearchCacheWorker do
  # ...
  @search_cache :search_cache

  def all_search_results_for(media_id) do
    case :ets.match_object(@search_cache, {{media_id, :"_"}, : "_"}) do
      [] ->
        nil
      all_objects ->
        all_objects
        |> Enum.map(fn {key, value} ->
          {elem(key, 1), elem(value, 1)}
        end)
    end
  end
end
```

:ets.match_object/2 返回一个包含所有匹配我们搜索条件的条目的列表。为了只返回每个条目的 search_expression 和结果，我们对结果列表进行映射，分别从条目键中获取第一个元素，从条目值中获取后者：

```
iex> SearchCacheWorker.all_search_results_for("1")
[{"other", "other phrase found on media 1 contents"}, {"media",
  "lorem ipsum media 1 contents"}]
```

最后，让我们使用每次缓存搜索结果时存储的 created_at 时间戳。这里的想法是创建一个 SearchCacheWorker.expired_search_results/0，它将返回所有创建时间超过 @expiration_secs 的搜索结果键。为此，我们必须使用 :ets.select/2 函数。此结构允许我们对正在模式匹配的每个条目应用守卫子句，这正是我们在这里想要的：

```
iex> expiration_secs = 60
60

iex> expiration_time = :os.system_time(:seconds) - expiration_secs
1518512436
```

```
iex> query = [
...> {
...> {:"$1", {:"$2", :_}},
...> [{:<, :"$2", {:const, expiration_time}}],
...> [:"$1"]
...> }
...> ]
[
{{:"$1", {:"$2", :_}}, [{:<, :"$2", {:const, 1518512436}}], [:"$1"]}
]
```

查询结构并不友好，但暂时不要担心。:ets.select 查询由一个或多个子句组成，每个子句都是一个三元素元组。让我们检查我们的单个子句查询。子句的第一个元素，{:"$1", {:"$2", :_}}，是我们将应用于表中每个条目的模式。这里，我们将 :"$1" 变量关联到 {media_id, search_expression} 键，将 :"$2" 关联到 created_at 元组元素。

第二个元素，[{:<, :"$2", {:const, expiration_time}}]，是守卫子句，告诉我们只会选择 :"$2" 值小于此作用域中定义的 expiration_time 的条目。子句元组的最后一个元素，[:"$1"]，显示了我们希望如何返回查询的值。如果我们想返回表中存储的完整条目，可以使用 :"$ _" 变量代替 :"$1"。

在查询就绪后，让我们使用它来查看哪些搜索结果已过期。首先，让我们缓存一个新的搜索结果，以确保查询正常工作：

```
iex> SearchCacheWorker.cache_search_result("7", "media", "really fresh media 7 contents")
{:ok, true}

iex> :ets.select(:search_cache, query)
[{"1","other"}, {"1","media"}, {"5","media"}]
```

:ets.select/2 调用没有返回我们刚刚缓存的搜索结果的键，所以一切正常。我们现在可以实现 SearchCacheWorker.expired_search_results/0 函数，它利用了我们刚刚创建的代码。我们将查询封装在私有 expired_search_results_query/1 中，只是为了整理一下：

```
$ cat apps/elixir_drip/lib/elixir_drip/storage/workers/
search_cache_worker.ex
defmodule ElixirDrip.Storage.Workers.SearchCacheWorker do
  # ...
  @search_cache :search_cache
```

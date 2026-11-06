
为了分析我们的应用程序，我们将使用 mix profile.fprof 任务：

```
$ mix profile.fprof -e
'ElixirDrip.Storage.store("177VXwhSuI7vjzkZOU7uvowAZzO", "test.txt", "$",
"some file content")'
```

```
Warmup...
Reading trace data...
End of trace!
Processing data...
Creating output...
Done!
                CNT    ACC
(ms)      OWN (ms)
Total                                   4023
2047.954     28.076
:fprof.apply_start_stop/4              0
2047.954     0.015
anonymous fn/0 in :elixir_compiler_1.__FILE__/1   1
2047.935     0.004
ElixirDrip.Storage.store/4            1
2047.931     0.016
ElixirDrip.Instrumenter.observe_duration/2   1
2047.837     0.010
anonymous fn/4 in ElixirDrip.Storage.store/4   1
2047.658     0.020
:suspend                               14
2019.878     0.000
ElixirDrip.Storage.Media.create_initial_changeset/4   1
2006.250     0.038
Process.sleep/1                        1
2000.888     0.020
DBConnection.run/3                     4
38.715      0.047
DBConnection.run_begin/3              2
37.645      0.017
ElixirDrip.Repo.insert/1              1
35.661      0.004
ElixirDrip.Repo.insert/2              2
35.657      0.008
Ecto.Repo.Schema.insert/4             2
35.653      0.009
# ...
```

我们裁剪了此命令的输出以便于阅读。例如，运行此任务实际上会创建一个上传任务，因为我们在生产环境中运行被分析的代码。

为了分析 store 函数，我们传递了 -e 参数，其中包含将被求值的代码。

分析器首先进行预热，然后进行分析本身，最后对生成的数据进行分析，结果打印到终端。

分析器进行预热是为了确保生成的数据尽可能接近真实请求。在此阶段，分析器在不进行任何测量的情况下运行代码。这很重要，因为第一次运行可能需要执行一些初始设置，例如连接到数据库，从而使第二次运行（即被测量的运行）可以使用第一次运行所做的设置。

现在让我们分析前一个代码块中最重要的部分：分析器的输出。输出由一个表格组成，其中第一列包含被测量的函数名称，其余列包含与该函数对应的数据。第一个数据列称为 CNT，包含在分析期间该函数被调用的次数。然后，我们有两个与时间测量相关的列：ACC 和 OWN，均以毫秒为单位。ACC 列包含在该函数中花费的总时间，而 OWN 列包含在该函数中花费的时间，不考虑其调用的函数所花费的时间。

![](img/c636f4cfb07f25f1db535995e50706ed_550_0.png)

默认情况下，profile.fprof 任务的输出按 ACC 时间排序，但我们也可以通过向 Mix 任务传递 --sort own 选项按 OWN 时间排序。

查看此表格，很清楚问题所在：我们有从大约 20 ms 到大约 38 ms 的巨大跳跃。这是使用合成示例的副产品，因为在现实世界中，我们可能不会看到两个函数之间花费时间如此明显的分界。另一个表明这是合成示例的线索是 ACC 和 OWN 时间之间的巨大差异。由于我们刚刚让 create_initial_changeset 函数中的进程休眠，这会使进程进入挂起状态，因此我们看不到它具有很大的 OWN 时间跨度。然而，在真实世界的示例中，您还应关注 OWN 列中显示的时间，它会迅速指出哪些函数在内部花费了最多的时间（不考虑对其他函数的调用）。

让我们再次运行分析器，但这次传递 --callers 选项。这将为我们提供有关时间花费的更详细视图（以生产系统额外负载为代价）。让我们分析这次运行的输出：

```
$ mix profile.fprof --callers -e
'ElixirDrip.Storage.store("177VXwhSuI7vjzkZOU7uvowAZzO", "test.txt", "$",
"some file content")'
Warmup...
# ...
ElixirDrip.Instrumenter.observe_duration/2              1      2024.454
0.016
anonymous fn/4 in ElixirDrip.Storage.store/4              1      2024.454
0.016 <--
    ElixirDrip.Storage.Media.create_initial_changeset/4    1      2004.626
0.027
    ElixirDrip.Repo.insert/1                               1      14.972
0.005
    ElixirDrip.Storage.get_owner/1                         1      4.518
0.002
    Ecto.Changeset.put_assoc/3                             1      0.312
0.004
ElixirDrip.Storage.Workers.QueueWorker.enqueue/2          1      0.010
0.001

anonymous fn/4 in ElixirDrip.Storage.store/4              1      2004.626
0.027
    ElixirDrip.Storage.Media.create_initial_changeset/4    1      2004.626
0.027 <--
    Process.sleep/1                                        1      2000.753
0.033
    ElixirDrip.Storage.Media.create_changeset/2           1      1.870
0.024
    KsuId.generate/0                                      1      1.141
0.043
    ElixirDrip.Storage.Media.generate_storage_key/2       1      0.572
0.012
    ElixirDrip.Storage.Providers.Encryption.Simple.generate_ 1      0.263
0.003

# ...
```

正如我们在第一次运行中所做的那样，我们裁剪了此命令的输出。当传递此选项时，我们会为第一次运行中看到的每个被分析函数获得一个专用块。在右侧，我们可以看到每个块上都有一个箭头，它表示当前块的标记函数。块中其余所有数字都相对于带有箭头的行。例如，在第一个块中，我们可以看到标记的函数是 `ElixirDrip.Storage.store`。在此分析器中，我们只有一个调用者，因此所有在 `store` 函数中花费的时间都来自该单个调用者。

更进一步，在 `store` 函数调用的函数中，我们可以详细地看到时间花费在哪里。我们可以观察到对 `ElixirDrip.Storage.Media.create_initial_changeset` 的调用花费了大约两秒，而下一个最长的调用是对 `ElixirDrip.Repo.insert` 的调用，花费了大约 15 毫秒。

再次，与第一个示例一样，存在巨大差距，在真实的分析会话中不太可能找到这种巨大差异。尽管如此，我们可以看到问题必须存在于 `ElixirDrip.Storage.Media.create_initial_changeset` 中。由于我们现在使用了 `--callers` 选项，我们现在可以继续分析此函数专用的块，我们在第一个块之后显示了它。在这个第二个块中，我们可以看到对于 `create_initial_changeset` 函数调用的函数，大部分时间（再次，大约两秒）花费在调用 `Process.sleep` 函数上。此函数调用之后，该块中最长的条目属于 `ElixirDrip.Storage.Media.create_changeset` 函数，花费了大约 2 毫秒。因此，我们找到了罪魁祸首！下一步将是修复它，然后密切监控它以尝试评估问题是否确实已解决。如果没有，调查将重新开始，但现在系统不同，这将导致不同的分析结果。

我们现在已经完成了一次分析会话，并学习了如何使用 `fprof` 分析器来分析代码的行为。然而，`fprof` 并不是随 Mix 一起提供的唯一分析器。我们还拥有 `eprof` 和 `cprof`，它们与 `fprof` 一样，都是 Erlang 分析器。您应该使用最适合您将进行的分析类型的分析器。`eprof` 分析器是基于时间的分析器（如 `fprof`），但以不同的方式呈现信息。除了每个函数的总调用次数外，它还显示每个函数花费的时间百分比，并显示每个函数的每次函数调用的微秒数。`cprof` 分析器仅显示每个被分析函数的函数调用次数，这本质上是我们之前在 `fprof` 输出中看到的第一列。如果您只对该指标感兴趣，使用 `cprof` 而不是 `fprof` 是有利的，因为它对生产系统的影响会更低。我们建议您查看这些 Mix 任务的官方文档，以了解所有可用选项。

![](img/c636f4cfb07f25f1db535995e50706ed_552_0.png)

`fprof` 的文档可在 https://hexdocs.pm/mix/Mix.Tasks.Profile.Fprof.html 获取，`eprof` 的文档可在 https://hexdocs.pm/mix/master/Mix.Tasks.Profile.Eprof.html 获取，`cprof` 的文档可在 https://hexdocs.pm/mix/Mix.Tasks.Profile.Cprof.html 获取。

我们已经到达了分析部分的结尾。现在让我们继续下一个主题，在那里我们将学习如何跟踪我们的实时应用程序。

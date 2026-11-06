```
{Step1Module, :start_link,
  [
    Step1Module.PAEC81181,
      [ # two subscriptions for each process
        {StartModule.PB6AD0F42, [max_demand: 6, min_demand: 4]},
        {StartModule.PDD3D519D, [max_demand: 6, min_demand: 4]}
      ]
  ], :permanent, 5000, :worker, [Step1Module]},
{"Elixir.Step1Module.PAEC1AE45", ...},
{"Elixir.Step1Module.PAEC5EBB3", ...},

# five Step2Module processes subscribing
# to the previous three stage processes
{"Elixir.Step2Module.P08E72352",
{Step2Module, :start_link,
  [
    Step2Module.P08E72352,
      [ # three subscriptions for each process
        {Step1Module.PAEC81181, [max_demand: 10, min_demand: 1]},
        {Step1Module.PAEC1AE45, [max_demand: 10, min_demand: 1]},
        {Step1Module.PAEC5EBB3, [max_demand: 10, min_demand: 1]}
      ]
  ], :permanent, 5000, :worker, [Step2Module]},
{"Elixir.Step2Module.P1470075A", ...},
{"Elixir.Step2Module.P9200FBE7", ...},
{"Elixir.Step2Module.PD4EB8A27", ...},
{"Elixir.Step2Module.P7A005C99", ...},

# one FinishModule process subscribing
# to the previous five stage processes
{"Elixir.FinishModule.PA2194E2F",
{FinishModule, :start_link,
  [
    FinishModule.PA2194E2F,
      [
        {Step2Module.P08E72352, [max_demand: 10, min_demand: 1]},
        {Step2Module.P1470075A, [max_demand: 10, min_demand: 1]},
        {Step2Module.P9200FBE7, [max_demand: 10, min_demand: 1]},
        {Step2Module.PD4EB8A27, [max_demand: 10, min_demand: 1]},
        {Step2Module.P7A005C99, [max_demand: 10, min_demand: 1]}
      ]
  ], :permanent, 5000, :worker, [FinishModule]}
]
```
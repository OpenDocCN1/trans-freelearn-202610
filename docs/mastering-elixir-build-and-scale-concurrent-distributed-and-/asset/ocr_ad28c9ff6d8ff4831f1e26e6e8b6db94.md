Similar to what happens with Mox, there's extra work to be done when your tests (or the code you're testing) are spawning new processes. In that case, the adapter can be changed from :manual to :shared mode, or by setting an explicit allowance on a certain process. Please refer to the adapters documentation, available at https://hexdocs.pm/ecto/Ecto.Adapters.SQL.Sandbox.html, for more information on how to configure the adapter for these cases.

The last noteworthy part of this test is the usage of @tag attributes. Tags allow us to add metadata to each test (or a group of tests), which can then be used to only run (or stop from running) a certain group of tests. In this case, we're tagging this test with :repository, since it interacts with the repository. With this in place we can, for instance, configure our local development to run all but the repository tests, and only run the full test suite in a CI (continuous integration) environment. We can run all tests except the repository tests with:

```
$ MIX_ENV=test mix test --exclude repository:true
Excluding tags: [repository: "true"]
.....
Finished in 0.1 seconds
7 tests, 0 failures, 1 skipped
```

As we can see, we had one test skipped, which is the repository test we just created. Conversely, if we just wanted to run the repository tests, we could pass the option --only repository:true. At this point, we're placing the :repository tag on the test itself. When the test module starts to contain more tests, we also want those tests to be tagged as well. To this end, we can use the @moduletag directive, which will place the given tag in all tests of the current module:

```
$ cat apps/elixir_drip/test/elixir_drip/storage/storage_repo_test.exs
defmodule ElixirDrip.StorageRepoTest do
  use ExUnit.Case, async: true

  @moduletag :repository

  # ...
end
```

With this we reach the end of the section dedicated to functions with side-effects. Let's now explore a very neat feature of Elixir: testing the documentation.
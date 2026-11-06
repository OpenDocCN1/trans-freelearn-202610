
```
# ...
end
```

Note that after changing this, we'll need to run mix deps.get to fetch this new dependency. The last step on setting up Mox is to define the mocks that we'll have across our tests. Unlike other testing frameworks, using Mox you need to do this in one place instead of per test. The rationale is that you shouldn't be generating dynamic modules in your tests to serve as a mock, but instead define them in one central place; for instance, in test_helper.exs or in a setup_all block, if for some reason you need to do it in the test module. In this example, we'll do it in test_helper.exs:

```
$ cat apps/elixir_drip/test/test_helper.exs
ExUnit.start()

Mox.defmock(ElixirDrip.Storage.Providers.GoogleCloudStorageMock, for:
ElixirDrip.Behaviours.StorageProvider)
```

We're using Mox.defmock/2 to define a mock with the name given as the first argument, which is based on the behaviour passed in the :for option of the second argument. Mox doesn't allow the creation of mocks without linking them to a behaviour. While this may feel like unnecessary work at first, remember that this will ensure the API of the mock and the real implementation don't drift away. Picking up on the explicitness that's present across Elixir's design and philosophy, Mox wants you to define explicit contracts, which both your actual implementation and your mock will have to abide to. This way, you'll not get to the point where your tests are all passing but your application is not working, which usually happens because your tests are abusing mocks and live in a different reality than your actual application.

![](img/c636f4cfb07f25f1db535995e50706ed_425_0.png)

We have already created the ElixirDrip.Behaviours.StorageProvider behaviour in Chapter 2, Innards of an Elixir Project.

If we hadn't already done this, we would need to create the behaviour, make the real implementation adopt that behaviour, and only then make this change in test_helper.exs. We're done with the groundwork. Let's now check the test itself:

```
$ cat apps/elixir_drip/test/elixir_drip/storage/pipeline/
remote_storage_test.exs
defmodule ElixirDrip.Storage.Pipeline.RemoteStorageTest do
  use ExUnit.Case, async: true
  import Mox

  alias ElixirDrip.Storage.Providers.GoogleCloudStorageMock
```

```
@subject ElixirDrip.Storage.Pipeline.RemoteStorage

setup :verify_on_exit!

test "on upload it calls the GoogleCloudStorage client with the right arguments" do
  expect(GoogleCloudStorageMock, :upload, fn "test_key", "test_content" -> {:ok, :uploaded} end)

  task = %{type: :upload, media: %{id: "test_id", storage_key: "test_key"}, content: "test_content"}
  @subject.handle_events([task], nil, nil)
end
end
```

The aim of this test is to check that our dependency is called with the right arguments. It's then the responsibility of the unit tests of that other module to check that it behaves correctly when given the right arguments. First of all, we're importing Mox, and then calling `setup :verify_on_exit!`. This setup call will make Mox verify that all the expectations defined in the current process were met, right before the process exits. Then, on the test itself, we're using the `expect` function (imported from Mox) to set an expectation on the mock we've created earlier. The first argument is the module we're setting the expectation on, the second argument is the name of the function, and the last argument is a lambda, which will be invoked once the mock receives this function call.

![](img/c636f4cfb07f25f1db535995e50706ed_426_0.png)

If you want to set an expectation where your mock will receive that function call more than once, you can provide another argument to the `expect` function, defining how many times this function will be called on the mock. For instance, extending our last example, if we expected the `:upload` function to be called three times in our mock, we would use: `expect(GoogleCloudStorageMock, :upload, 3, fn "test_key", "test_content" -> {:ok, :uploaded} end)`

We're using pattern matching on the lambda's arguments to verify that the mock is being called with the right arguments. If the mock is called with different arguments, the match will fail, which in turn will make the test fail. If the mock is called with the right arguments, it'll return whatever is the result of the lambda. In this case, we're simply returning `{:ok, :uploaded}`, so that the pattern match works in the module being tested, making the test pass.

After setting this expectation, we're simply creating the necessary test data, where we define that this is an upload task, and then passing it on to the function we're testing on the subject. Looking at this test, it's very clear what are the expectations set on this test, hence making it easy to understand its purpose. If instead of setting an expectation you just wanted to stub that function call, you can just call the `Mox.stub/3` function. To check this and other functions available on Mox, please take a look at its documentation, available at https://hexdocs.pm/mox/Mox.html.

Note that we still have `async: true` at the top of the file, meaning that, although we're using mocks, this test will still run concurrently. However, this may not always be the case. Particularly, if the code you're testing is spawning new processes (for instance, spawning a GenServer), you won't be able to set expectations on function calls made by these processes. In these cases, you can configure Mox to work in global mode, where every process can access the expectation set by a different process (and stop running the tests concurrently with `async: false`), or make an explicit allowance for a certain process to access the expectations (allowing the tests to still run concurrently). Check the documentation mentioned earlier for details on how to use these modes.

We've now seen how to create functional mocks in Elixir, using Mox. Before wrapping up this section of testing functions with side-effects, let's talk about testing functions that interact with a repository.

## Testing interactions with the repository separately

We'll now explore how to run the tests that interact with the application's repository separately. At the beginning of this chapter, we created some tests for the `%Media{}` struct, and seen how we can test the `changeset` logic without ever touching the database. Now, we'll see how to actually run the tests that hit the database.

A common question that's asked every once in a while is how to create a mock for the repository, so that we can test the functions that interact with it independently. While the repository is indeed another dependency, we think that creating a mock for it makes no sense, because in the repository tests, we want to verify that our commands and queries work as expected. We have already tested our business logic in `changeset` tests, so now we want to make sure everything is glued together as it should be. If we were to mock the repository, we would then still need to create integration tests that would verify this, so what's the advantage of creating the mock in this scenario?

One of the main reasons people want to avoid hitting the database in their tests is that it slows down the test suite, creating a slower, and hence less productive, feedback cycle. However, we're able to run tests that interact with the database concurrently, greatly removing the reason for this concern. Let's now see how we can run tests that hit the database concurrently. First, we need to configure our database pool in the test environment to use a special sandbox adapter:

```
$ cat apps/elixir_drip/config/test.exs
use Mix.Config

# ...

config :elixir_drip, ElixirDrip.Repo,
  pool: Ecto.Adapters.SQL.Sandbox
```

This adapter will wrap each connection to the database in a transaction, hence creating a safe mechanism to run concurrent tests against the database. Although the name of the adapter contains SQL, the concurrent mode can't be used for all SQL databases. For now, it can only be used with PostgreSQL, as using it with MySQL may lead to deadlocks, creating a brittle test suite. Now, we need to update our test_helper.exs, to configure the pool mode:

```
$ cat apps/elixir_drip/test/test_helper.exs
# ...
Ecto.Adapters.SQL.Sandbox.mode(ElixirDrip.Repo, :manual)
```

This will configure the pool mode to :manual, which means that in each test we will be explicitly checking out a connection. Let's now see the test itself. Although we will test queries against the %Media{} schema, due to our design being based on bounded contexts, the repository interactions for this schema live in the Storage context. Hence, our test will be against the Storage module. Also, note that we're separating the repository tests into a different file, allowing us to clearly see which tests are interacting with the repository (and the database) and which tests are plain unit tests. Let's check the code for this test:

```
$ cat apps/elixir_drip/test/elixir_drip/storage/storage_repo_test.exs
defmodule ElixirDrip.StorageRepoTest do
  use ExUnit.Case, async: true

  @subject ElixirDrip.Storage

  setup do
    :ok = Ecto.Adapters.SQL.Sandbox.checkout(ElixirDrip.Repo)
  end

  describe "when the user already uploaded some media" do
```

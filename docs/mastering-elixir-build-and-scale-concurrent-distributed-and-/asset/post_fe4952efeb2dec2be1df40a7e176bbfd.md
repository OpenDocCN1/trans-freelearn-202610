
```
setup [:create_user, :create_media]

@tag :repository
test "finds the media by ID", %{user: user, media: media} do
  {:ok, :download_enqueued, retrieved_media} =
    @subject.retrieve(user.id, media.id)

  expected_media_id = media.id
  assert %{full_path: "$", id: ^expected_media_id, name:
    "test.txt"} = retrieved_media
end
end

defp create_user(_context) do
  {:ok, user} =
    ElixirDrip.Accounts.create_user(%{
      username: "test",
      password: "12345678",
      email: "test@test.com"
    })

  {:ok, user: user}
end

defp create_media(%{user: user}) do
  {:ok, :upload_enqueued, media} = @subject.store(user.id,
  "test.txt", "$", "some cool content")
  {:ok, media: media}
end
end
```

First of all, we have the `setup` block, where we're checking out a connection from the pool, as we've discussed earlier. This way our current process will be the owner of this connection to the database. In the rest of the test, we're using the techniques we've already learned throughout this chapter to create descriptive tests. We use a `describe` block to set the context for a certain test (or group of tests), and then use `setup` inside it to arrange the necessary state for the test to run. In this case, that means creating a new user in the database, and then a new media entry associated with that user. Then, the test itself is verifying that we're able to fetch a media entry based on its ID and the associated user ID. Lastly, we use the `assert` macro to verify that the record we've just got out of the database is the one we're expecting.

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

## Testing the documentation

Throughout this book we've been saying how documentation is a first-class citizen of Elixir, which is a great help when onboarding new people to the language (or even for experienced developers being introduced to an application). One of the key enablers for this is the ability to embed tests within the documentation for a function, which will then ensure that the documentation is kept up to date with the code.

Pretty much every developer recognizes the value of concise and descriptive documentation, as it greatly helps the understanding of a function or a module without having to jump all over a project. The problem with documentation is that it's difficult to maintain, as the code may be updated without doing the corresponding change to the documentation, which creates outdated and misleading documentation. This caused certain communities to just stop writing documentation, as it's better having none than having deceitful documentation. To tackle this issue, but still keep the great value added by proper documentation, Elixir and ExUnit allow us to embed snippets of code in the documentation, which will get executed and compared against the expected output you define in the documentation as well. The name of this feature is doctest.

To exemplify the creation of doctest, we'll add documentation to the function we've tested previously, ElixirDrip.Storage.Media.is_valid_path?/1:

```
$ cat apps/elixir_drip/lib/elixir_drip/storage/media.ex
defmodule ElixirDrip.Storage.Media do
  # ...

  @doc"""
  Verifies if the provided `path` is valid or not.

  ## Examples

      iex> ElixirDrip.Storage.Media.is_valid_path?("$ /abc")
      {:ok, :valid}

When we provide an invalid path (that either doesn't start with `$` or ends with `/`), it returns {:error, :invalid_path}

      iex> ElixirDrip.Storage.Media.is_valid_path?("$ /abc/")
      {:error, :invalid_path}

      iex> ElixirDrip.Storage.Media.is_valid_path?("/abc")
      {:error, :invalid_path}
  """
  def is_valid_path?(path) when is_binary(path) do
    # ...
```

```
end
  # ...
  end
```

In here we're writing documentation with the @doc module attribute, as usual. Then, inside it we introduce some lines that begin with iex>, on which we append the code that we want to test. The line right after this one contains the expected output. ExUnit will then extract the code that's after iex>, run it, and compare it with what's written in the next line. To have these tests running as part of our test suite, we need to add the following to this module's test file:

```
$ cat apps/elixir_drip/test/elixir_drip/storage/media_test.exs
defmodule ElixirDrip.Storage.MediaTest do
  # ...

  doctest @subject

  # ...
end
```

The @subject module attribute was already defined previously, and contains the name of the module that we're testing. By adding this line, ExUnit will run the doctest snippets we've just created as part of our test suite. Let's run them now:

```
$ MIX_ENV=test mix test
..........

Finished in 12.6 seconds
3 doctests, 7 tests, 0 failures
```

As we can see, we now have 3 doctests as part of our test suite. Let's now intentionally make the test fail, so that we can observe its output in that case. We modified the documentation to expect a return value of {:ok, :other_atom}. Let's check its output:

```
$ MIX_ENV=test mix test
......

1) doctest ElixirDrip.Storage.Media.is_valid_path?/1 (1)
(ElixirDrip.Storage.MediaTest)
  test/elixir_drip/storage/media_test.exs:5
Doctest failed
code: ElixirDrip.Storage.Media.is_valid_path?("$ /abc") === {:ok, :other_atom}
left: {:ok, :valid}
stacktrace:
  lib/elixir_drip/storage/media.ex:74: ElixirDrip.Storage.Media
```

```
(module)
..
Finished in 0.3 seconds
3 doctests, 7 tests, 1 failure
```

In this case, the test fails, and we get back a nice error message, making it easy to understand what's going on and correct what's necessary. With this we've finished this subsection on doctest, and also the broader section on unit tests. Let's now move on to the next section, where we'll learn how to write integration tests.

## Integration testing

Having covered unit tests, let's now move on to integration tests. In this type of testing, we're interested in asserting that all the different layers of our application are working together as expected. This is usually done by testing at the edges of your system, letting all the components in your application interact with each other, and then validating that the response is the anticipated one.

There are other types of testing, such as end-to-end testing, which go even a step further and test the system from the perspective of the end user. In our case, this would be through a browser, clicking through the buttons and links inside our application, and then asserting that the user sees the expected information. While we don't cover this type of testing in this book, much of what we'll talk about in this section also apply to end-to-end testing.

In this kind of test, where we're observing the application from the outside, we don't want to test every possible state combination that may occur. That should be done at the unit test level, checking every possible branch for the subject under test. At the integration level, we test the main paths that the user may take.

As stated previously, these tests will be done at the boundary of our system. This means that we will hit the endpoints just as the browser would, which will then go through the Phoenix pipelines and controllers, interact with the elixir_drip umbrella application, and finally return a response by rendering a view. As an example of an integration test, we will be creating a test with a GET /files request, which will go to the index action of the file controller. Let's check it now:

```
$ cat apps/elixir_drip_web/test/elixir_drip_web/controllers/
file_controller_test.exs
defmodule ElixirDripWeb.FileControllerTest do
  use ElixirDripWeb.ConnCase, async: true
```

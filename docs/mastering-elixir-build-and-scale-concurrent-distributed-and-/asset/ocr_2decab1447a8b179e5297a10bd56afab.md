# Testing the documentation

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
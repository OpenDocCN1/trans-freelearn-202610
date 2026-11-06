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

![](bbox=[0.12, 0.6, 0.2, 0.7])

If you want to set an expectation where your mock will receive that function call more than once, you can provide another argument to the `expect` function, defining how many times this function will be called on the mock. For instance, extending our last example, if we expected the `:upload` function to be called three times in our mock, we would use: `expect(GoogleCloudStorageMock, :upload, 3, fn "test_key", "test_content" -> {:ok, :uploaded} end)`

We're using pattern matching on the lambda's arguments to verify that the mock is being called with the right arguments. If the mock is called with different arguments, the match will fail, which in turn will make the test fail. If the mock is called with the right arguments, it'll return whatever is the result of the lambda. In this case, we're simply returning `{:ok, :uploaded}`, so that the pattern match works in the module being tested, making the test pass.
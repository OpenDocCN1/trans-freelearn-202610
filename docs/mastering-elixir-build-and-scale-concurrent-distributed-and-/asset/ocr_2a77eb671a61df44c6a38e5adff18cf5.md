```
spec:
  # ...
  containers:
    - image: gcr.io/intense-talent-188323/elixir-drip-prod:${BUILD_TAG}
```

With this Travis CI setup, every time we push a commit to the master branch, Travis CI will perform all these steps for us. In the end, we'll have an updated version of ElixirDrip up and running on the cloud. In the next section, we'll connect the deployed Elixir nodes so we can better use the media cache we've previously implemented.

# Connecting the deployed Elixir nodes

Our objective in this section is to connect every ElixirDrip node running in Kubernetes. With the current setup, each Elixir node is running on its own pod, without even trying to connect to other pods.

When we introduced the Kubernetes deployment template, we briefly talked about the /health endpoint used by the readiness and liveness probe. Let's use this endpoint to get information about the node that replies to our HTTP GET requests. Here you can find the Phoenix controller that will handle the requests to /health:

```
$ cat apps/elixir_drip_web/lib/elixir_drip_web/controllers/health_controller.ex
defmodule ElixirDripWeb.HealthController do
  @moduledoc false

  use ElixirDripWeb, :controller

  def health(conn, _params) do
    {_, timestamp} = Timex.format(DateTime.utc_now, "%FT%T%:z", :strftime)

    {:ok, hostname} = :inet.gethostname

    json(conn, %{
      ok: timestamp,
      hostname: to_string(hostname),
      node: Node.self(),
      connected_to: Node.list()
    })
  end
end
```
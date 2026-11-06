![](bbox=[0.12, 0.2, 0.22, 0.3])

Even if we set the @url value to "${EXTERNAL_API_URL}", the error would still happen, because this value would never be replaced by the actual URL value (the @url value would always be "${EXTERNAL_API_URL}"). Only the config.exs configuration entries are placed in the sys.config file by Distillery. At runtime, when Distillery detects the REPLACE_OS_VARS flag, it replaces all the ${...} placeholders that exist in the sys.config file with the values of the actual environment variables.

After understanding how we can safely use environment variables to configure the application at runtime, let's actually build and run the release!

# Creating the release

To build the release, we need to simply run the release Mix task with mix release. Since in our rel/config.exs file we set default_environment: Mix.env(), the release default environment is given by the MIX_ENV environment variable.

In the following snippet, we are building the release. We're setting the elixir_drip umbrella app version in its mix.exs file as 0.0.8, so the release becomes elixir_drip:0.0.8 (elixir_drip is the release name and 0.0.8 results from setting the release version as current_version(:elixir_drip) in the rel/config.exs file):

```
$ MIX_ENV=prod mix release
==> elixir_drip
# ...
Generated elixir_drip app
==> elixir_drip_web
Compiling 12 files (.ex)
Generated elixir_drip_web app

==> Assembling release..
==> Building release elixir_drip:0.0.8 using environment prod
==> Including ERTS 9.3.1 from /usr/local/lib/erlang/erts-9.3.1
==> Packaging release..
==> Release successfully built!
  You can run it in one of the following ways:
  Interactive: _build/prod/rel/elixir_drip/bin/elixir_drip console
  Foreground: _build/prod/rel/elixir_drip/bin/elixir_drip foreground
  Daemon: _build/prod/rel/elixir_drip/bin/elixir_drip start
```

If you look inside the resulting *.tar.gz release archive, you will find four folders (three if you don't include the ERTS): bin, erts-<version>, lib, and releases/<app-version>.
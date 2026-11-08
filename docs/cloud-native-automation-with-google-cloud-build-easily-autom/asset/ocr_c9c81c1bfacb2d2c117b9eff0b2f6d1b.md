Not only do we see `env` defined in our build step stanza, but we also see `timeout` defined. This field is fairly straightforward; you define a period of time after which if the build step has not been completed, it will fail the entire build.

In the example we are working with, the build step that creates a VM has a timeout set to 240 seconds or 4 minutes. Timeouts are defined as a duration; the default unit is seconds if left unspecified. However, you can specify additional units via hours (h) and minutes (m).

Additionally, you have the ability to combine multiple units of time into a single timeout value. For example, `timeout: 2h10m45s` would be 2 hours, 10 minutes, and 45 seconds.

> **Note**
> At the time of publication, the maximum timeout for a build is 24 hours.

Timeouts are useful at the individual build step level if you have known build steps that depend on calling unreliable external systems or APIs, or when you are concerned about minimizing costs given that builds are priced per vCPU build minute utilized – especially when utilizing larger workers.

Similar to `env` and `secretEnv`, `timeout` can be defined not only for an individual build step but also at a global level for the entire build, which we will review in the *Configuring build-wide specifications* section.

Now, let’s move on to the `dir` field. When a build executes, by default, the current working directory is set to `/workspace`. This is backed by a Docker volume mounted at this path by default, with the same volume provided to each build step that runs:

![](bbox=[0.22, 0.815, 0.78, 0.885])

Figure 4.5 – Each build step will get access to the same volume mounted at /workspace

For many build steps, using `/workspace` as your working directory is sufficient. However, there are certain tools that need to run from a specific working directory, perhaps where certain configuration files are located that are passed to the tool.
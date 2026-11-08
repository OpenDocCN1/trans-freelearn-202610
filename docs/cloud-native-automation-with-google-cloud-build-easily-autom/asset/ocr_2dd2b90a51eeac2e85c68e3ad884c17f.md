In our example build configuration, the args field for each of the build steps is defined in flow style:

```
args: ['build',
    '-t',
    'us-central1-docker.pkg.dev/${PROJECT_ID}/my-docker-repo/myimage',
    '.']
```

For smaller sets of arguments, flow style provides a simple and readable way to write args. You can also separate each item in flow style with a line break for additional readability.

However, for larger, more complex sets of arguments, utilizing block style might be easier to write. The same set of args fields from the previous example written in block style would look like the following:

```
args:
  - 'build'
  - '-t'
  - 'us-central1-docker.pkg.dev/${PROJECT_ID}/my-docker-repo/myimage'
  - '.'
```

Feel free to tweak the cloudbuild.yaml example with different styles of defining args and rerun the build.

> Note
> 
> The maximum number of args you can provide to a build step is 100, with the sum of characters in a single args field capped at 4,000.

Regardless of which way you write your arguments, there are two ways to use args to kick off having a builder carry out tasks:

+   - If the cloud builder provides an entrypoint, the values in args will be utilized as arguments for that entrypoint.
+   - If the cloud builder does not provide an entrypoint, the first value in args will be utilized as the cloud builder's entrypoint with the remaining values used as arguments.
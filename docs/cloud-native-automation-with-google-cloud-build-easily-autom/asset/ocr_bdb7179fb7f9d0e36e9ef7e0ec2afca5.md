# Defining the relationships between individual build steps

Build steps rarely live in a vacuum and must understand their relationships to the other build steps in the build. A build step might need to write to files that a subsequent build step needs to access. Multiple build steps might need to run in parallel, wait for other build steps to complete before beginning their task, or even kick off a separate build.

Let’s discuss two specific parameters that you could utilize to define these relationships between the build steps:

+   - `volumes`
+   - `waitFor`

volumes are the means by which data is persisted on the Cloud Build worker between build steps. We have already reviewed the default volume, workspace/, as the default working directory and the easiest place to persist files between build steps.

However, if your tooling needs to write files to a specific location, or expects files to be in a specific directory, you can also create your own volumes and mount them to persist files in your own custom locations.

An example of this might be when utilizing SSH keys in your build. Tooling, such as git, might expect SSH keys to be in /.ssh, which is under the home directory of the user that the container uses to run.

Specifying your own bespoke volumes to persist data between build steps would look like this:

```
steps:
  - name: 'gcr.io/cloud-builders/git'
    secretEnv: ['SSH_KEY']
    entrypoint: 'bash'
    args:
      - -c
      - |
        echo "$$SSH_KEY" >> /root/.ssh/id_rsa
        chmod 400 /root/.ssh/id_rsa
        cp known_hosts.github /root/.ssh/known_hosts
    volumes:
      - name: 'ssh'
        path: /root/.ssh

  # Clone the repository
  - name: 'gcr.io/cloud-builders/git'
```
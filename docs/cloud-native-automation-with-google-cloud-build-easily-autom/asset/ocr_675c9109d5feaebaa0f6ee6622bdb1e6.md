```
chmod 400 /root/.ssh/id_rsa
ssh-keyscan gitlab.com > /root/.ssh/known_hosts
entrypoint: bash
secretEnv:
  - GITLAB_SSH_KEY
...
availableSecrets:
  secretManager:
    - versionName: projects/282614098327/secrets/GITLAB_SSH_KEY/versions/1
      env: GITLAB_SSH_KEY
```

The secretManager key is used to identify that the Secret Manager service is to be utilized. versionName is the unique identifier to the secret and version that is to be retrieved. The value of the secret is then stored in the environment variable, GITLAB_SSH_KEY, which can be used in the build steps of the pipeline. In this build step, notice how the variable named GITLAB_SSH_KEY is associated with the secretEnv key, so it can be leveraged in the args build argument.

Specifying availableSecrets in Cloud Build does not automatically grant access to the secret that is specified. The roles/secretmanager.secretAccessor role binding for the specific secret must be specified – in this case, referencing the service account to be used by the Cloud Build trigger.

![](bbox=[0.18, 0.53, 0.73, 0.86])

Figure 6.4 – How a secret is accessed by a build and build step
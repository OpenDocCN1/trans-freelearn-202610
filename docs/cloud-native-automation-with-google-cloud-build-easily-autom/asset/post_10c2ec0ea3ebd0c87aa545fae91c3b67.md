
## Integrations with source code management platforms

The source repository is a critical component of a trigger because it’s dependent on the resources in the repository and a snapshot of the files so that builds can reference a particular set of source files. Cloud Build supports a growing list of SCM platforms. Following is a list of currently supported SCM platforms and how they are integrated with Cloud Build (that is, trigger types and how repositories can be accessed).

| SCM Platform | Trigger type | Access |
|---|---|---|
| Cloud Source Repositories | Native | Service account permissions |
| GitHub | Native | GitHub auth and Cloud Build GitHub app |
| GitHub Enterprise | Native | GitHub app and enterprise config |
| GitLab | Webhook | SSH key in Secret Manager |
| GitLab Enterprise | Native | GitLab auth and enterprise config |
| Bitbucket Cloud | Native | SSH key in Secret Manager |
| Bitbucket Server | Native | Tokens in Secret Manager and enterprise config |
| Bitbucket Data Center | Native | Tokens in Secret Manager and enterprise config |

Table 5.1 – SCM platform trigger type and access configurations

Each of these platforms, as noted, may have a native trigger or webhook trigger configuration. The authentication and authorization of repositories may also depend on the platform. When there are secret resources to be managed, native Google Secret Manager integration is available to help manage secrets. For GitHub, via the GitHub app, it is also possible to update the build status on the commit in the repository.

Once a trigger is configured to listen on a specific regex of the repository branch or tag, a matching commit will initiate a build.

The following exports an existing trigger:

```
$ gcloud builds triggers export \
    packt-cloudbuild-trigger \
    --destination export.yaml
```

Here’s the output of the generated export.yaml configuration:

```
autodetect: true
createTime: '2022-01-04T20:26:51.731858991Z'
description: Invokes a build every time code is pushed to any branch
github:
  name: packt-cloudbuild
  owner: kenthua
  push:
    branch: ^main$
id: 2aad99c0-2e3c-4a43-9b24-cbdaa12cbaf8
name: packt-cloudbuild-trigger
tags:
- cloudbuild-sample-branch-trigger
```

The code is committed to the branch that the trigger is listening to:

```
$ git push origin main
```

Example output of the push follows. Note the commit hash of 5023f77:

```
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 4 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 411 bytes | 411.00 KiB/s, done.
Total 3 (delta 1), reused 0 (delta 0)
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To github.com:kenthua/packt-cloudbuild.git
  17ab064..5023f77  main -> main
```

Given the preceding example trigger, once code is pushed to the main branch, a build will trigger and pull the source. Listing the build history using the CLI outputs the following:

```
$ gcloud builds list
```

The following is a truncated list of the build in progress. You are able to query the status and track the build as needed:

```
ID: a50ec9b7-84a1-467e-b5ca-d89af472f783
CREATE_TIME: 2022-02-10T01:48:44+00:00
DURATION: 2M4S
SOURCE: -
IMAGES: -
STATUS: WORKING
```

The following command retrieves the logs of the specified build:

```
$ gcloud builds log a50ec9b7-84a1-467e-b5ca-d89af472f783
```

The following is a truncated log output of the build:

```
----------------------------------------------- REMOTE BUILD
OUTPUT -----------------------------------------------
starting build "a50ec9b7-84a1-467e-b5ca-d89af472f783"

FETCHSOURCE
hint: Using 'master' as the name for the initial branch. This
default branch name
hint: is subject to change. To configure the initial branch
name to use in all
hint: of your new repositories, which will suppress this
warning, call:
hint:
hint:   git config --global init.defaultBranch <name>
hint:
hint: Names commonly chosen instead of 'master' are 'main',
 'trunk' and
hint: 'development'. The just-created branch can be renamed via
this command:
hint:
hint:   git branch -m <name>
Initialized empty Git repository in /workspace/.git/
From https://github.com/kenthua/packt-cloudbuild
* branch               5023f77a3913ac3c97ec0bacac7257fa29b60a5a  -> FETCH_HEAD
HEAD is now at 5023f77 update cloud build manifest;
BUILD
Starting Step #0
```

Note that it fetches the commit hash 5023f77, which was pushed to the SCM repository. Cloud Build itself doesn’t have the build resources as it was triggered by the SCM event, but the build still needs to fetch the commit from the repository to proceed with the build.

## Defining your own triggers

Pre-built integrations are very helpful for getting started quickly. However, they are not the only way to integrate with Cloud Build. You can leverage other mechanisms such as a manual or webhook trigger in order to integrate with existing systems or platforms that currently are not yet pre-built in Cloud Build.

## Webhook triggers

An example we will be running through is a webhook trigger and we will be using GitLab as an example. Native integration with GitLab offerings are on the way as noted earlier in the chapter.

![](img/2fe9576240c146c49e3a21d5e7f6af00_95_0.png)

Figure 5.3 – An example of webhook trigger creation

Let's break down the webhook URL:

- Domain: `cloudbuild.googleapis.com`
- Google Project ID
- Trigger Name: `packt-cloudbuild-gitlab-trigger`
- Type: webhook
- Key: generated API key
- Secret: generated secret when specifying **New** or an existing secret such as a password

In the example URL noted previously, we have a standard domain, the context path consisting of the Google project ID, and a trigger name. The unique elements protecting the URL are the key and secret. The URL is expecting an HTTP POST method and optional data, which can be parsed to be used in the pipeline.

## Advanced

## Substitution variables

Substitutions allow re-use of a cloudbuild.yaml file with different variable values. Use bash string manipulation to combine variables and bindings to access arbitrary data in the JSON payload of the webhook. Learn more

![](img/2fe9576240c146c49e3a21d5e7f6af00_96_0.png)

Figure 5.4 – Substitution variables

Substitution variables can be used to allow for flexibility of common pipeline files with variables that can be passed in to make the pipeline more flexible. In this case, we are using a substitution variable to extract the GitLab commit ID to be used in the pipeline. As noted previously, in the case of GitLab, the response consists of repository metadata and most importantly the commit ID, which can be used in the pipeline to retrieve the specific commit (https://docs.gitlab.com/ee/user/project/integrations/webhook_events.html#push-events).

In a webhook configuration or when trying to access repositories that do not have pre-built integration, Cloud Build is not able to access the repository for a cloudbuild.yaml configuration. In this case, a trigger must have the pipeline configuration inline. This can be edited via the GCP console UI. If using the gcloud CLI, it can be added using an --inline-config switch, which references a cloudbuild.yaml configuration in the filesystem.

## Configuration

Type

Cloud Build configuration file (yaml or json)

Dockerfile

Buildpacks

Location

Repository
Select a repository above

Inline
Write inline YAML

OPEN EDITOR

Figure 5.5 – Build configuration

Selecting **OPEN EDITOR** in the preceding figure will open up an editor for specifying the pipeline configuration. The following screenshot is an example configuration snippet of the inline `cloudbuild.yaml` for the trigger when adding through the GCP console UI.

## Edit inline configuration

Write the build config file using the YAML syntax. Formatting and comments won't be stored.

```
steps:
# first, setup SSH:
# 1- save the SSH key from Secret Manager to a file
# 2- add the host key to the known_hosts file
- name: gcr.io/cloud-builders/git
  args:
  - '-c'
  - |
    echo "$GITLAB_SSH_KEY" > /root/.ssh/id_rsa
    chmod 400 /root/.ssh/id_rsa
    ssh-keyscan gitlab.com > /root/.ssh/known_hosts
  entrypoint: bash
  secretEnv:
  - SSHKEY
  volumes:
  - name: ssh
    path: /root/.ssh
# second, clone the repository
- name: gcr.io/cloud-builders/git
  args:
  - clone
  - '-n'
  - 'git@gitlab.com:.../packt-cloudbuild'
  - .
  volumes:
  - name: ssh
    path: /root/.ssh
# third, checkout the specific commit that invoked t
```

Figure 5.6 – Inline build configuration editor

![](img/2fe9576240c146c49e3a21d5e7f6af00_97_0.png)

![](img/2fe9576240c146c49e3a21d5e7f6af00_97_1.png)

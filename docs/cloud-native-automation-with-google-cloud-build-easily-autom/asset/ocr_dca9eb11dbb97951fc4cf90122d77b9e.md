The last build step is terraform apply, for applying the outlined changes:

```
$ cat << EOF >> cloudbuild.yaml
- id: 'tf apply'
  name: 'hashicorp/terraform:1.0.0'
  env:
  - "TF_IN_AUTOMATION=true"
  entrypoint: 'sh'
  args:
  - '-c'
  - |
    terraform apply -input=false terraform.tfplan
EOF
```

Once we have our Cloud Build configuration available to us, we can manually execute the build by running the command used in previous chapters to submit a build. The default Cloud Build service account will be used:

```
$ gcloud builds submit --region us-west1
```

A successful build will yield the following truncated output with a SUCCESS status:

```
...
Step #2 - "tf apply": Apply complete! Resources: 3 added, 0 changed, 0 destroyed.
Finished Step #2 - "tf apply"
PUSH
DONE
...
STATUS: SUCCESS
```

**Note**
In automation, the TF_IN_AUTOMATION variable is set so that some instructions are truncated and manual instructions are not presented. For example, after executing terraform plan automatically with the variable set, the following statement is not outputted to the console: “To perform exactly these actions...”
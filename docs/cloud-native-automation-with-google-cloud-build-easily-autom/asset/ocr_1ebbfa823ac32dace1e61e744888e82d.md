While it is possible to enable Binary Authorization on a Cloud Run service level, it is recommended to enforce that Cloud Run services have Binary Authorization enabled at a higher level, such as an Organization, a Folder, or a Project.

![](bbox=[0.2, 0.18, 0.78, 0.46])

Figure 9.12 – Hierarchy of a Cloud Run service resource

If it is to be set only on a per-service basis, any developer with Cloud Run permissions can just as easily remove Binary Authorization from their service.

You can enable this at an Organization, Folder, or Project level with the following gcloud command, adding the respective resource ID to the --organization, --folder, or --project flag:

```
$ gcloud resource-manager org-policies allow run.
allowedBinaryAuthorizationPolicies default
```

With this in place, you now can create a Cloud Run service using gcloud run deploy while passing the --binary-authorization=default flag.

Similar to the example laid out in Chapter 8, Securing Software Delivery to GKE with Cloud Build you can now update your admission policies by running the following commands:

```
$ gcloud container binauthz policy export > /tmp/policy.yaml
```

This grabs the default admission policy with Binary Authorization enabled. You can edit this policy file by adding a block like the following into the policy file under the defaultAdmissionRule:

```
...
requireAttestationsBy:
```
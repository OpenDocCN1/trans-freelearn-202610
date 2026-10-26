
With the *installPlanApproval: Automatic* specification in the operator subscriptions [36], operator deployments are typically set to be automatically upgraded when its catalog source is refreshed. A choice of a manual approval implies that a cluster admin manually decides when the upgrade is acceptable. Note that new versions of Cloud Pak for Data operators are expected to still tolerate currently existing installed versions of the operand deployments and support new versions at the same time. Hence, in the same cluster, a fresh installation of the new version of the service could be deployed, while another namespace continues to retain the old version, and both get managed by the same operator.

Operands can be set to be auto-upgraded as well. This would mean that when the operator is upgraded, as part of its reconcile loop, it automatically upgrades existing operands as well to the latest version supported by that operator.

The easiest approach is to configure the fully automated mechanisms and adopt the continuous delivery paradigm. With this approach, all cluster administrator needs to do is to simply ensure that the catalog source is updated (manually if air-gapped). Operators introduced by that updated catalog service would then get automatically upgraded to the latest version and those operators would, in turn, automatically upgrade the operands they manage.

For situations where a specific version of an operand is required and upgrades need to be handled manually, a process called "pinning" can be adopted. Pinning operands to a version works even when the operator concerned has been upgraded. This is done by explicitly providing a version in the custom resource spec for that service in an instance namespace. Only when the version is manually altered would the operator trigger the upgrade of the operand deployments to the new version requested. If no version is specified, then the operator assumes that the operand deployments always need to be at the latest release level and automatically ensures that to be the case.

## Scale-out

Out of the box, operand deployments of services are configured to use specific compute resources and replica settings. Most services support a T-shirt sizing configuration that provides for the scaling out and scaling up of the various Pod microservices. In many cases, scale-out is also done to improve high availability with multiple replicas for deployments and improved performance with increased load balancing.

27. **Alerting:** https://www.ibm.com/docs/en/cloud-paks/cp-data/4.0?topic=platform-monitoring-alerting

28. **Custom monitors:** https://www.ibm.com/docs/en/cloud-paks/cp-data/4.0?topic=alerting-custom-monitors

29. **Compute resources for containers in Kubernetes:** https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/

30. **Service operator subscriptions:** https://www.ibm.com/docs/en/cloud-paks/cp-data/4.0?topic=tasks-creating-operator-subscriptions#preinstall-operator-subscriptions__svc-subscriptions

31. **Kubernetes namespace resource quotas:** https://kubernetes.io/docs/tasks/administer-cluster/manage-resources/quota-memory-cpu-namespace/

32. **Storage volumes:** https://www.ibm.com/docs/en/cloud-paks/cp-data/4.0?topic=platform-managing-storage-volumes

33. **Storage volumes API:** https://www.ibm.com/docs/en/cloud-paks/cp-data/4.0?topic=resources-volumes-api

34. **Kubernetes namespaces:** https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/

35. **OpenShift projects:** https://docs.openshift.com/container-platform/4.7/applications/projects/working-with-projects.html

36. **Operator subscription InstallPlans:** https://www.ibm.com/docs/en/cloud-paks/cp-data/4.0?topic=tasks-creating-operator-subscriptions#preinstall-operator-subscriptions__install-plan

37. **Scaling services:** https://www.ibm.com/docs/en/cloud-paks/cp-data/4.0?topic=cluster-scaling-services

38. **Backup and restore:** https://www.ibm.com/docs/en/cloud-paks/cp-data/4.0?topic=cluster-backing-up-restoring-your-project

39. **Offline backup via filesystem copy:** https://www.ibm.com/docs/en/cloud-paks/cp-data/4.0?topic=bu-backing-up-file-system-local-repository-object-store

40. **Backing up with a snapshot:** https://www.ibm.com/docs/en/cloud-paks/cp-data/4.0?topic=up-backing-file-system-portworx

## Development practices

IBM requires all products to be developed and evaluated using strict secure engineering practices, and the services delivered for Cloud Pak for Data are no exception. With such offerings also being operated in an as-a-service fashion, accessible to the general public, security is of paramount importance right from the design of the software.

The security practices and reports are independently reviewed by the IBM hybrid cloud **Business Information Security Office (BISO)** prior to any release of the software.

This section describes some of the important practices used to develop security-hardened services in Cloud Pak for Data.

## Vulnerability detection

A key initiative is to detect possible security weak points or potential areas that a malicious operator could leverage to introduce man-in-the-middle attacks, access sensitive data, or inject malware. Given that quite a bit of interaction with Cloud Pak for Data is through a web browser or HTTP-based clients in general, care is taken to ensure protection from both outside and in-cluster influences:

- **Threat modeling**: This is a process to analyze where the system is most vulnerable to attack, by building out inter-service and intra-service detailed data flow diagrams, understanding potential attack vectors, as well as evaluating whether existing protection is deemed sufficient. The output of the threat modeling exercise is used to drive the development of additional security constructs or to refactor service interactions to mitigate potential threats.

- **Security quality assurance and code reviews**: Additional tests are developed by a team focused on security and operating independently from typical functional development and quality assurance squads. This effort also involves validation of cross-service/cross-product integrations and a thorough evaluation of general system reliability using **Chaos Monkey** style testing approaches (that is, mechanisms to generate random failures in the system to validate resiliency).

This allows for a better objective evaluation of a proposed software delivery against required security guidelines. As part of this initiative, this team also conducts independent code reviews and approval of any mitigations proposed by the service development teams.

Hence, it is expected that all OpenShift clusters are able to pull images from a designated *Enterprise* container registry in their own private network. It is expected that pull secrets are made available to the service accounts used by Cloud Pak for Data.

## The role of a bastion node

Bastion nodes are typically machines that are deployed outside of the enterprise's private network, with controlled access to repositories and registries. Such hosts facilitate access to container images, utilities, scripts, and other content from IBM and make them available to OpenShift clusters for provisioning.

One approach is to download all images as `tar.gz` files and transfer these files into the private network where they can be loaded into an appropriate container registry. However, a more convenient approach is to use the container registry to directly replicate container images from ICR into the enterprise's own registry, from such a bastion node. This is made possible because the bastion node has access to the internet and to the enterprise's own data center or at least the target registry for the duration. A bastion node's role may extend beyond just the initial installation since this approach is quite useful for pulling or replicating newer container images, including those that patch any serious vulnerabilities.

In this section, we covered how IBM Cloud Pak for Data containerized software is made available for customers from a trusted location for customers to ensure its provenance, and how these images can be introduced into the customer's own registry. In the next section, we will look at what it means to operate Cloud Pak for Data securely.

## Secure operations in a shared environment

In traditional systems, many applications share the same operating system, be it a virtual machine or bare metal. However, for security reasons, these applications are typically never granted access to the operating system or run as *root*. Since many programs, from different products or vendors, would be operating on the same machine, care is also taken to isolate each of these operating system processes from each other.

While it has been common to simply spin up a separate virtual machine for each application to completely isolate them, it was also expensive to operate and could possibly lead to a waste of compute resources. Containers have proven to be much less expensive in the long run, but there is a trade-off with regard to less isolation of workloads. In this section, we will look at how the stack enables security from the ground up, starting with the host operating system in the cluster, to OpenShift security constructs as well as how Cloud Pak for Data leverages Kubernetes access control primitives to enforce controls on all Services operating in that cluster.

> **Note**
> 
> The instance API key is not available for some services in the Cloud Pak for Data v3.5 release.

Cloud Pak for Data provides user interfaces and APIs [28] to generate, re-generate, or delete a platform API key or an instance API key.

# The Secrets API and working with vaults

The Secrets API [29] in Cloud Pak for Data provides the ability to access sensitive credentials from configured vaults. This helps avoid the need to use plain-text credentials as part of developer-authored content such as notebooks or scripts. Using a vault, Enterprises can also control policies to rotate keys and credentials independently.

Since Cloud Pak for Data's Secrets API retrieves the contents of the vault only when required, it will get the latest password or key. This also prevents the need for users to update passwords in many places.

Cloud Pak for Data will continue to introduce integrations with different vault vendors and technologies as part of its roadmap.

# Meeting compliance requirements

Regulatory compliance and enterprise policies require stringent monitoring of systems to ensure that they are not compromised. There are usually multiple standards to adhere to and different audit techniques that are required. Red Hat and IBM Cloud Pak for Data provide tools and mechanisms for security architects to design and configure the system as per the policies of their organization as well as to audit usage.

In this section, we will take a brief look at the capabilities that help enterprises plan for compliance.

# Configuring the operating environment for compliance

For Cloud Pak for Data, Red Hat's Enterprise Linux for the host and OpenShift Container Platform represent the technological foundation. Hence, to ensure compliance, the foundation needs to be properly configured. A few key aspects of the technical stack that help in compliance readiness are listed as follows:

+   - RHEL supports a strong security and compliance posture such as setting up for the **Federal Information Processing Standard (FIPS)** publication 140-2. Several advanced techniques exist to secure RHEL hosts [30], including tools, scanners, and best practice guidance.
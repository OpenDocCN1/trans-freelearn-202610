We have just seen here the installation of InSpec, then the writing of InSpec tests, and finally how to use it with its command line to test the compliance of Azure infrastructure. In the next section, we’ll look at another aspect of security with the protection of sensitive data using the secrets manager, Vault, from HashiCorp.

# # Keeping sensitive data safe with HashiCorp Vault

Today, when we talk about security in information systems, the most expected topic is the protection of sensitive data between different components of the system. This sensitive data that needs to be protected includes server access passwords, database connections, **application programming interface (API) authentication tokens**, and application user accounts. Indeed, many security attacks occur because this type of data is decrypted in the source code of applications or in poorly protected files that are exposed to local workstations. Many known tools can be used to secure this sensitive data, such as these:

+   - KeyPass (`https://keepass.info/`)
+   - LastPass (`https://www.lastpass.com/`)
+   - Ansible Vault, the use of which we discussed _Chapter 3, Using Ansible for Configuring IaaS Infrastructure_
+   - Vault from HashiCorp

Also, cloud providers offer their own secrets protection services such as the following:

+   - Azure Key Vault: `https://azure.microsoft.com/en-us/services/ key-vault/`
+   - **Key Management Service (KMS)** for Google Cloud Platform: `https://cloud. google.com/kms/`
+   - AWS Secrets Manager for AWS: `https://aws.amazon.com/secrets- manager/?nc1=h_ls`

Out of all of the tools we have mentioned, we’ll look at the use of **Vault** from HashiCorp, which is free and open source and can be installed on any type of OS as well as on Kubernetes.

These are the main features and benefits of Vault:

+   - It allows the storage of static secrets as well as dynamic secrets.
+   - It also has a system for rotating and revoking secrets.
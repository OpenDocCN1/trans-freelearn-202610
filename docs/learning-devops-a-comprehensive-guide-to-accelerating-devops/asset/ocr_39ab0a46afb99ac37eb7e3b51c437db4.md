+   - It allows data to be encrypted and decrypted without having to store it.
+   - It also has a web interface that allows the management of secrets.
+   - It integrates with a multitude of authentication systems.
+   - All secrets are stored in a single centralized tool.
+   - It allows you to be independent of your architecture by being accessible with all major cloud providers, Kubernetes, or even on internal data centers (on-premises).

> Note
> For more information on Vault features, see the product page at https : // www.vaultproject.io/docs/what-is-vault/index.html.

With all of these very interesting features, Vault is therefore a tool that I recommend for any company that wants to protect its sensitive information and integrate its secure accessibility into a CI/CD pipeline. Indeed, in addition to being very efficient for data protection, Vault integrates very well in CI/CD pipelines. These pipelines will be able to use this protected data when provisioning infrastructure and deploying applications.

After an overview of Vault, we’ll proceed with the installation of Vault on a local machine and use it for encrypting and decrypting data. We’ll also give an overview of the Vault user interface (UI), and finally, we’ll expose the process of how to retrieve data from Vault in Terraform.

# # Installing Vault locally

If you decide to use Vault, it is important to know that it is a tool that is responsible for the security of your sensitive infrastructure and application data. In practice, Vault is not just a tool, and before installing it in production, you need to understand its concepts and its different architectural topologies.

> Note
> To learn more about Vault architecture topologies, please refer to the documentation at https : //learn.hashicorp.com/vault/ operations/ops-reference-architecture.

The purpose of this chapter is therefore not to go into the details of the concepts and architecture of Vault but to explain the installation and use of Vault in development mode. In other words, we’ll install Vault on a local workstation to have a small instance that’s used for testing and development.
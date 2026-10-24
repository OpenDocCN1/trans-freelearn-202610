

## The following screenshot shows its execution:

![](img/d3344b39c888aab0b832625465f837c9_453_0.png)

Figure 14.10 – Vault get secret by its version

We can see in this output, in the Data section, version 1 of the key-value data we had during the first execution of the `kv put` command.

> **Note**
> For more information on the `kv get` command, read the documentation at `https://www.vaultproject.io/docs/commands/kv/get.html`.

We have just seen the use of the `kv get` Vault command to retrieve all or specific versions of the values of a secret; we’ll now briefly see how to use the Vault UI web interface for better management of secrets.

## Using the Vault UI web interface

One of the interesting features of Vault is that, apart from the client-side tool that allows you to perform all operations on the Vault server, Vault has a UI web interface that allows you to manage secrets, but more visually and graphically.

To open and use the Vault web interface to visualize the secrets that we have created with the client tool, we must follow these steps:

1. In a browser, enter the URL provided when starting the server—that is, `http://127.0.0.1:8200/ui`, which is the default local Vault URL.

2. In the authentication form, enter the token that was provided in the Terminal in the root token information, as shown in the following screenshot:

![](img/d3344b39c888aab0b832625465f837c9_454_0.png)

Figure 14-11 – Vault getting the root token

3. Click on the Sign In button to authenticate, as illustrated here:

![](img/d3344b39c888aab0b832625465f837c9_454_1.png)

Figure 14-12 – Vault UI sign-in

4. The home of the interface displays a list of secret paths, called **Secrets Engines**, containing secrets that have been stored, as illustrated in the following screenshot:

![](img/d3344b39c888aab0b832625465f837c9_455_0.png)

Figure 14.13 – Vault UI Secrets Engines

5. By clicking on each secret engine, you can see a list of secrets that have been saved. The following screenshot shows the secret engine page, which displays the secret we created on the command line in the previous section:

![](img/d3344b39c888aab0b832625465f837c9_455_1.png)

Figure 14.14 – Vault UI secret

6. By clicking on a specific secret, you can access a list of data that we have protected, with the possibility of viewing the values of each key in cleartext. We can also display the history of the content of a secret by selecting the desired version in the **History** drop-down menu, as illustrated in the following screenshot:

![](img/d3344b39c888aab0b832625465f837c9_456_0.png)

Figure 14.15 – Vault UI: reading secrets’ details

The Vault web interface also allows you to perform all management operations on secrets and other Vault components.

> **Note**
> If you want to know more about this web interface, read this article: https://www.hashicorp.com/resources/vault-oss-ui-introduction

We have seen that the Vault web interface is a very good alternative to the client tool that allows you to view and manage Vault elements. After this overview of Vault and its operations, I propose a small Vault use case that shows you how to get secrets in Terraform code.

## Getting Vault secrets in Terraform

As we have already seen in _Chapter 2, Provisioning Cloud Infrastructure with Terraform_, it is very important to protect the infrastructure configuration information that we write in Terraform code. One way to protect this sensitive data is to store it in a secrets manager such as Vault and recover it directly with Terraform dynamically.

Here is an example of Terraform code that allows you to retrieve the password of a VM that you want to provision from Vault. This Terraform code example is composed of three blocks, which are detailed as follows:

1. First, we use the Vault provider for configuring the Vault URL by executing the following code:

```hcl
provider "vault" {
    address = "http://127.0.0.1:8200" #Local Vault Url
}
```

The Vault provider is configured with the Vault server configuration and its authentication.

In our case, we configure the Vault server URL in the Terraform code, and for the authentication of a token, we’ll use an environment variable when running Terraform after an explanation of the code.

> Note
> For more details on the Terraform Vault provider and its configuration, see the following documentation:
> https://www.terraform.io/docs/providers/vault/index.html

2. Then, we add the Terraform data block, vault_generic_secret, which is used for retrieving a secret from a Vault server. The code is illustrated in the following snippet:

```hcl
data "vault_generic_secret" "vmadmin_account" {
    path = "secret/vmadmin"
}
```

This data block allows us to retrieve (in read-only mode) the content of a secret stored in Vault. Here, we ask Terraform to retrieve the secret that is in the secret/vmadmin Vault path that we created earlier in this section.

> Note
> For more details on vault_generic_secret data and its configuration, see the following documentation:
> https://www.terraform.io/docs/providers/vault/d/generic_secret.html

3. Finally, we add an output block to use the decrypted value of the secret, as follows:

```hcl
output "vmpassword" {
  value = "${data.vault_generic_secret.vmadmin_account.data["vmpassword"]}"
  sensitive = true
}
```

This block provides an example of the exploitation of the secret.

The `data.vault_generic_secret.vmadmin_account.data["vmpassword"]` expression is used to get the secret returned by the previously used data block. In the `data` array, we add the name of only those keys for which we need the encrypted values to be recovered. Also, this output is considered `sensitive` so that Terraform does not display its value in plaintext when it is executed.

> **Note**
> The complete Terraform source code is also available here:
> https://github.com/PacktPublishing/Learning-DevOps-Second-Edition/blob/main/CHAP14/vault/terraform_usevault/main.tf

We have finished writing the Terraform code; we'll now quickly execute it to see the recovery of the secret.

> **Note**
> For the execution of the code, which we have already detailed in _Chapter 2, Provisioning Cloud Infrastructure with Terraform_, we'll only quote the commands without further detailing them in this section.

To execute Terraform, we go in a Terminal to the folder that contains the Terraform code, and then we proceed in this order:

1. Export the `VAULT_TOKEN` environment variable with the value of the Vault token. In our development mode case, this token is provided at the start of the Vault server. The following command shows the export of this environment variable on a Linux OS:

```bash
export VAULT_TOKEN=xxxxxxxxxxxxxx
```

2. Then, we'll execute Terraform with these commands:

```bash
terraform init
terraform plan
terraform apply
```

Here is a quick summary of the details of these commands:

- The `terraform init` command initializes the context and downloads all necessary providers.
- The `terraform plan` command displays a preview of all changes that will be applied by Terraform.
- The `terraform apply` command applies all changes on the infrastructure and displays the output values.

> **Note**
> To learn all the details of the main Terraform commands and the Terraform life cycle, read _Chapter 2_ of this book, _Provisioning Cloud Infrastructure with Terraform_.

The following screenshot shows the execution of the `apply` command:

![](img/d3344b39c888aab0b832625465f837c9_459_0.png)

Figure 14.16 – Terraform sensitive output

We can see that the value in the Terraform output named `vmpassword` is not displayed in clear text in the Terminal.

3. Finally, we display the Terraform output value in **JavaScript Object Notation** (JSON) format with the `terraform output` command and the `-json` option, as follows:

![](img/d3344b39c888aab0b832625465f837c9_460_0.png)

Figure 14.17 – Terraform output command in JSON format

We see that Terraform has displayed the value of the key that was in the secret, which we had inserted in Vault. This value can now be used for any sensitive data that should not be stored in Terraform code, such as VM passwords.

> However, Be Careful
> We have protected our Terraform code by outsourcing all sensitive data to a secrets manager, but it should not be forgotten that Terraform stores all information—including data and output information—in the Terraform state file. It is therefore very important to protect it by storing the Terraform state file in a protected remote backend, as we saw in the Protecting the state file in a remote backend section of Chapter 2, Provisioning Cloud Infrastructure with Terraform.

In this section, we have studied the use of HashiCorp’s Vault, which is a secret data manager. We have seen how the installation on different OSes can be done manually and automatically. We used its command lines to protect and read data that we have protected inside. Then, we discussed how to manage secrets in Vault using its web interface. Finally, we wrote and executed Terraform code that uses the Vault provider and allows us to retrieve secrets that we stored in a Vault server.

## Summary

This chapter is dedicated to integrating security into DevOps practices. We presented three tools to verify and secure your data and cloud infrastructure. We discussed how to check the compliance of an Azure infrastructure using **InSpec** from Chef.

To do this and check infrastructure compliance, we installed InSpec and then detailed the writing of InSpec tests. We used its command lines to verify the compliance of Azure infrastructure.


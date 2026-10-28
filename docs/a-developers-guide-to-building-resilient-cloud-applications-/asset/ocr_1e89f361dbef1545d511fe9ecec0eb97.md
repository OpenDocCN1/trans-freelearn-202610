This process will take a few minutes to complete. If we want to simplify the creation of a Service Fabric cluster using interactive mode, we will use the portal. However, we can also create it using the Azure CLI. This is what we will look at in the next section.

# Exercise 2 – creating a Service Fabric cluster using the Azure CLI

Another option that we can use to create Azure Service Fabric clusters is the Azure CLI tool. You can open PowerShell if it is installed on your machine, or open Azure Cloud Shell using this link: https://shell.azure.com/.

You can find the entire sample command line on GitHub by following this link: https://github.com/PacktPublishing/A-Developer-s-Guide-to-Building-Resilient-Cloud-Applications-with-Azure/tree/main/Chapter5.

We will use PowerShell; we will type this command line to create a new cluster:

```
az sf cluster create -g packtrg -c hospitalmanagement -l estus --cluster-size 5 --vm-password Password#1234 --certificate-output-folder MyCertificates --certificate-subject-name hospitalmanagement
```

We can use Key Vault Certificate and a custom template to deploy a cluster as follows:

```
az sf cluster create -g packtrg -c hospitalmanagement -l estus--template-file template.json \ --parameter-file parameter.json --secret-identifier https://{KeyVault}.vault.azure.net:443/secrets/{MyCertificate}
```

To create a new Azure Service Fabric cluster, we can use one or more properties mentioned here:

```
az sf cluster create --resource-group [--cert-out-folder] [--cert-subject-name] [--certificate-file] [--certificate-password] [--cluster-name] [--cluster-size] [--location] [--os {UbuntuServer1604, WindowsServer1709, WindowsServer1709withContainers, WindowsServer1803withContainers, WindowsServer1809withContainers, WindowsServer2012R2Datacenter, WindowsServer2016Datacenter,
```
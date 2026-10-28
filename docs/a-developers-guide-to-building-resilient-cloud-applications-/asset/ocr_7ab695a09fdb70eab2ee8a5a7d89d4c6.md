# Primary node type details

Node types define the scale sets that will be used to manage your cluster. A Service Fabric cluster can consist of more than one node type. In that event, the cluster consists of one primary node type and one or more non-primary node types. Configure your primary node type here. Learn more about node types

Initial VM scale set capacity *

Node types *

Standard D2s v3
Select VM size

# Security

Service fabric requires a primary certificate to authenticate the nodes in a cluster and provide secure communication. This certificate must contain a private key and have a subject name that matches the domain used to access the cluster. You can create a new certificate now or select an existing one from an Azure Key Vault. The certificate must be stored in an Azure Key Vault in the same subscription and region as the cluster. Learn more about Service Fabric security

Key vault and primary certificate *

Select a certificate

Figure 5.10 – Step 2 – Create Service Fabric cluster

Now, in the Security blade, we’re required to select or create an Azure Key Vault resource where we will store certificates. So, let’s click on this option and we will create a new key vault. Let’s name this servicefabrickeycert, for example. We will select the same resource group as we selected for Service Fabric. After, we will click on the Create button. So, now the Azure portal will deploy this Azure Key Vault resource because it’s required in order to create the certificate and store it there.

![](bbox=[0.14, 0.56, 0.87, 0.8])

Figure 5.11 – Select certificate from Azure Key Vault

After the key vault is provisioned, you will see a message that says that it’s not enabled for deployment, so we need to click on the Edit access policies button. The key vault has to be enabled for deployment.
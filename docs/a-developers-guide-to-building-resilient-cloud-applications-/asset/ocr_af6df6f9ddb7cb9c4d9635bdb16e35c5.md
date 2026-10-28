To enable the key vault, we need to edit the access policies for the key vault by selecting the Enable access to Azure Virtual Machines for deployment option, located in the advanced access policies section, and clicking Save.

![](bbox=[0.13, 0.16, 0.845, 0.315])

Figure 5.12 – The edit access policies for the key vault and the certificate

# Important note

The certificate must be stored in Azure Key Vault in the same subscription and region as the cluster.

In the Node types tab, we can specify how many VMs we want in our scale set. As you can see, here we're using the value of five. Also, you can specify a single-node cluster.

# Create Service Fabric cluster

![](bbox=[0.14, 0.54, 0.845, 0.73])

Figure 5.13 – Step 3 – Create Service Fabric cluster – Node types tab
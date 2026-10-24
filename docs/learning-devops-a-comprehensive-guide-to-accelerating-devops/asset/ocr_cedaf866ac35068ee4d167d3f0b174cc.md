Before we start using Packer, we need first to create an Azure resource group that will
store the created VM image by the pipeline. In our lab, we named this Azure resource
group rg_images. The following screenshot shows the Azure resource group:

![](bbox=[0.3555, 0.1699, 0.6016, 0.2871])

Figure 8.1 – The Azure resource group for the Packer image

Now, we can write the code of the pipeline, which will execute Packer command lines on
the Packer template.

To do this, in the same folder as the Packer template, create a new YAML file that will
describe the steps of the pipeline, and then write three blocks of code, as follows:

+   1. The first step is to execute the `packer init` command:
+   
+   ```yaml
+   - script: packer init $(Build.SourcesDirectory)/CHAP08/
+     packer/.pkr.hcl
+     displayName: Packer init
+   ```

+   2. The second step is to validate the Packer template by running the `packer validate` command:
+   
+   ```yaml
+   - script: packer validate $(Build.SourcesDirectory)/
+     CHAP08/packer/.pkr.hcl
+     displayName: Packer validate template
+   ```

+   3. The last step is to build the image with Packer by running the `packer build` command:
+   
+   ```yaml
+   - script: packer build $(Build.SourcesDirectory)/CHAP08/
+     packer/.pkr.hcl
+     displayName: Packer build template
+     env:
+       PKR_VAR_clientid: $(PKR_VAR_clientid)
+       PKR_VAR_clientsecret: $(PKR_VAR_clientsecret)
+       PKR_VAR_subscriptionid: $(PKR_VAR_subscriptionid)
+       PKR_VAR_tenantid: $(PKR_VAR_tenantid)
+   ```
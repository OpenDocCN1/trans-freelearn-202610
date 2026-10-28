
In this chapter, we are going to cover the following main topics:

- Preparing for Contributor (PaaS) exploit scenarios
- Attacking storage accounts
- Pillaging keys, secrets, and certificates from key vaults
- Leveraging web apps for lateral movement and escalation
- Extracting credentials from automation accounts

## Preparing for Contributor (PaaS) exploit scenarios

To follow along with the exercises in this chapter, you will need to set up a user with Contributor permissions and some vulnerable workload configurations in your own Azure subscription. As in previous chapters, we have automated this using a PowerShell script that you can run from Azure Cloud Shell. Before proceeding, ensure that you have run the clean-up scripts to remove resources from previous chapters. This will help us avoid any script execution exceptions.

Here are the tasks that we will complete in this exercise:

- **Task 1:** Create a test user account with Contributor privileges.
- **Task 2:** Deploy vulnerable workloads for all the scenarios.

Let's get started:

1. Open a web browser and browse to the Azure portal at `https://portal.azure.com`. Sign in with your `azureadmin` credentials.
2. In the Azure portal, click on the **Cloud Shell** icon in the top-right corner. Select **PowerShell**:

![](img/4fa338e84e0a590967a142dd798ba4e2_226_0.png)

Figure 6.1 – Click the icon to open Cloud Shell

3. In the PowerShell session within the Cloud Shell pane, run the following command to download a script to create a user account with Contributor permissions and set up the required vulnerable workloads. Verify the download by listing the contents of the directory:

```
PS C:\> Invoke-WebRequest http://bit.ly/contributor-paas-scenarios -O contributor-paas-scenarios.ps1
PS C:\> Get-ChildItem
```

Here is the destination GitHub URL that the shortened URL points to: `https://raw.githubusercontent.com/PacktPublishing/Penetration-Testing-Azure-for-Ethical-Hackers/main/chapter-6/resources/contributor-paas-scenario.ps1`.

Here is a screenshot of what it looks like:

![](img/4fa338e84e0a590967a142dd798ba4e2_227_0.png)

Figure 6.2 – Download the Contributor PaaS scenario script

4. Run the downloaded script to provision the objects and resources needed for the exercises in this chapter using the following command:

```
PS C:\> ./contributor-paas-scenarios.ps1
```

When prompted to enter a password, enter `myPassword123` and press *Enter*. Wait for the script deployment to complete. The deployment may take about eight minutes to complete:

![](img/4fa338e84e0a590967a142dd798ba4e2_227_1.png)

Figure 6.3 – Run the Contributor PaaS scenario script

### What does the script do?

The script creates a user account with Contributor permissions in the Azure subscription. The script also sets up the resources and objects shown in Figure 6.4:

- A web app with an associated managed identity
- Azure SQL Database and Azure Storage with firewall rules
- Azure Key Vault with secrets, keys, and certificates
- An Azure Automation account
- An Ubuntu Linux VM:

![](img/4fa338e84e0a590967a142dd798ba4e2_228_0.png)

Figure 6.4 – Contributor PaaS exploit scenario

5. After the script has finished, the key information that you will need for the rest of this exercise will be displayed in the output section. Copy the information into a Notepad document for later reference. There are four values that you will need from the output section:

Azure Contributor User: This is the Azure administrator username with Contributor permissions to the Azure subscription.

Azure Contributor User Password: This is the password of the Azure Contributor administrator user.

**Linux VM FQDN:** The FQDN of the Linux VM that will be used to mount an image in one of the exercises. This is needed because WSL as implemented on the PentestVM does not yet support this.

**Linux VM Password:** This is the password of the Linux VM that will be used to mount an image in one of the exercises:

![](img/4fa338e84e0a590967a142dd798ba4e2_229_0.png)

Figure 6.5 – Obtain the script output

You have now successfully created the resources that we will work with in later exercises in this chapter. The first of the resources that we will explore in this chapter is storage accounts.

## Attacking storage accounts

Azure storage accounts are the primary data storage solution in Azure. Other Azure services, such as App Service and Container Registry, rely on it for data storage in the backend. The solution itself offers five services that can be used to store different datasets, including unstructured application data objects (Azure Blob Storage), semi-structured application data in a NoSQL store (Azure Table Storage), managed file shares in the cloud (Azure Files), a messaging store for reliable messaging (Azure Queue Storage), and a data lake for big data workloads (Azure Data Lake Storage Gen2). As you can imagine, this service is a prime target for attackers! For our purposes in this chapter, we will focus on Azure Blob Storage and Azure Files.

![](img/4fa338e84e0a590967a142dd798ba4e2_229_1.png)

Figure 6.6 – Azure Storage

The Contributor role has permissions to read and modify any configuration on the management plane of this service (except for assigning permissions to other users), but it does not have inherent access to read or modify data in the data plane based on RBAC permissions (Figure 6.7). This may sound confusing, but note that the Contributor role policy document does not specify any `dataActions` permissions:

```json
"permissions": [
  {
    "actions": [
      "*"
    ],
    "notActions": [
      "Microsoft.Authorization/*/Delete",
      "Microsoft.Authorization/*/Write",
      "Microsoft.Authorization/elevateAccess/Action",
      "Microsoft.Blueprint/blueprintAssignments/write",
      "Microsoft.Blueprint/blueprintAssignments/delete",
      "Microsoft.Compute/galleries/share/action"
    ],
    "dataActions": [],
    "notDataActions": []
  }
]
```

![](img/4fa338e84e0a590967a142dd798ba4e2_230_0.png)

Figure 6.7 – Azure Contributor permissions

An Azure AD identity needs to be granted additional `dataActions` permissions such as the ones listed in Figure 6.8 to access the data plane with an Azure AD access token. As Microsoft moves more toward this pattern of exposing service data plane access through Azure RBAC, there will be more opportunities to exploit the data plane of services in Azure without having to pivot to another option:

| Role | Description |
|---|---|
| Storage Blob Data Contributor | Allows for read, write and delete access to Azure Storage blob containers and data |
| Storage Blob Data Owner | Allows for full access to Azure Storage blob containers and data, including assigning POSIX access control. |
| Storage Blob Data Reader | Allows for read access to Azure Storage blob containers and data |
| Storage File Data SMB Share Contributor | Allows for read, write, and delete access in Azure Storage file shares over SMB |
| Storage File Data SMB Share Elevated Contributor | Allows for read, write, delete and modify NTFS permission access in Azure Storage file shares over SMB |
| Storage File Data SMB Share Reader | Allows for read access to Azure File Share over SMB |
| Storage Queue Data Contributor | Allows for read, write, and delete access to Azure Storage queues and queue messages |
| Storage Queue Data Message Processor | Allows for peek, receive, and delete access to Azure Storage queue messages |
| Storage Queue Data Message Sender | Allows for sending of Azure Storage queue messages |
| Storage Queue Data Reader | Allows for read access to Azure Storage queues and queue messages |

Figure 6.8 – Azure Storage-related roles

Here are the steps to complete the stated tasks:

1. In the Pentest VM, right-click the Start button and click on Windows PowerShell (Admin).

2. Authenticate to Azure PowerShell using the following command:

```
Connect-AzAccount
```

3. When prompted to authenticate, use the Azure Contributor User value (from the script output) as the username and the Azure Contributor User Password value (also from the script output) as the password:

![](img/4fa338e84e0a590967a142dd798ba4e2_232_0.png)

Figure 6.9 – Output from authenticating as contributoruser

You are now authenticated as the contributoruser account. This account is assigned the Contributor role in the Azure subscription. This account will be used in subsequent exercises to exploit various weaknesses in the Azure environment. Keep the PowerShell session open for the next exercise.

4. Import the MicroBurst module into your PowerShell session with the following commands:

```
cd .\MicroBurst\
```

```
Import-Module .\MicroBurst.psm1
```

5. The MicroBurst toolkit will import different PowerShell functions depending on which PowerShell modules you have installed on your system. Ignore the warning about the MSOnline module not being installed:

![](img/4fa338e84e0a590967a142dd798ba4e2_232_1.png)

Figure 6.10 – Import the MicroBurst PowerShell module

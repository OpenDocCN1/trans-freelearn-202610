
```powershell
Import-Module AzureADPreview
Get-Module AzureADPreview | select Name,Version
Name              Version
----              -------
AzureADPreview 2.0.2.138
```

To review the sign-in logs for interesting events, we can use the Get-AzureADAuditSignInLogs cmdlet as shown in the following commands (this command requires a native work/school Azure AD user account and not a Microsoft account). The following commands will filter the sign-in logs for successful Azure Portal or Azure PowerShell sign-ins in the time range specified (the date can be modified to the date before your current date to get the results for the past 24 hours). The output of these commands provides great information that we can use to verify the users of Azure management tools, which usually means that they have some level of access to Azure resources:

```powershell
# First connect to Azure AD
Connect-AzureAD

# Get successful Azure Portal sign-ins in the time range
# (greater than the specified date)
Get-AzureADAuditSignInLogs -Filter "appDisplayName eq 'Azure Portal' and createdDateTime gt $((Get-Date).AddDays(-1).ToString('yyyy-MM-dd')) and status/errorCode eq 0"

# Get successful Azure PowerShell sign-ins in the time range
# (greater than the specified date)
Get-AzureADAuditSignInLogs -Filter "appDisplayName eq 'Microsoft Azure PowerShell' and createdDateTime gt $((Get-Date).AddDays(-1).ToString('yyyy-MM-dd')) and status/errorCode eq 0"
```

We can filter this further to get the properties that are useful to show us the security configurations of the user accounts. This information can be useful if we need to look to bypass defenses:

```powershell
# Get interesting authentication properties for successful
# Azure Portal sign-ins
Get-AzureADAuditSignInLogs -Filter "appDisplayName eq 'Azure Portal' and createdDateTime gt $((Get-Date).AddDays(-1).ToString('yyyy-MM-dd')) and status/errorCode eq 0" | Select-Object UserPrincipalName, MfaDetail, AppliedConditionalAccessPolicies
```

Here is a sample output of this command:

![](img/4fa338e84e0a590967a142dd798ba4e2_278_0.png)

Figure 7.3 – PowerShell command results for sign-in auditing

The following Azure AD built-in roles have the permissions needed to read this information: **Security Administrator**, **Security Reader**, and **Report Reader**. Additionally, any custom Azure AD role that has the following permissions could also be used:

- `microsoft.directory/groups/allProperties/allTasks` – Grants permission to read and update group properties in Azure AD
- `microsoft.directory/signInReports/allProperties/read` – Grants permission to read sign-in reports

It is also worth noting that there are opportunities to move laterally from Azure AD to other cloud applications that may be using Azure AD as their identity provider if your pentest scope covers that. You can discover other applications that have a trust relationship with Azure AD by reviewing the **Enterprise Applications** in Azure AD. You can use this section to find out the users authorized for the applications and the groups that they belong to. If the access is granted via groups, you can add yourself to the groups to gain permissions to those cloud applications.

# Path 2 – Resetting user passwords

Another path that could be exploited to move laterally to another Azure subscription is to reset the password of a user who has permissions to another Azure subscription and logging in as that user. This path is less preferred as it impacts the existing access of a user and is more intrusive. It easily raises an alarm if a user is suddenly not able to log in to their account with the password that they have always used, and the audit logs reflect the recent password change event.

> **Important note**
>
> Microsoft has great documentation on how this feature works within Azure AD. Please refer to the following URL for additional information: `https://docs.microsoft.com/en-us/azure/role-based-access-control/elevate-access-global-admin#how-does-elevated-access-work`.

Now that we have seen how we can escalate rights, let's go through a practical example.

# Hands-on exercise – Preparing for the Global Administrator/Owner exploit scenarios

This hands-on exercise will prepare us for the rest of the exercises in this chapter. To follow along with the scenarios that we will cover in this chapter, you will need to set up a user with Global Administrator permissions in your Azure AD tenant and another user with the Owner permissions role in your Azure subscription. We have automated this using a PowerShell script that you can run from the Azure Cloud Shell. Here are the tasks that we will complete in this exercise:

1. Open a web browser and browse to the Azure Portal (`https://portal.azure.com`). Sign in with the `azureadmin` credentials.

2. In the Azure Portal, click on the Cloud Shell icon in the top-right corner of the Azure Portal. Select *PowerShell*:

![](img/4fa338e84e0a590967a142dd798ba4e2_282_0.png)

*Figure 7.7 – Click the icon to open Cloud Shell*

3. In the PowerShell session within the Cloud Shell pane, run the following command to download a script to create a user account with Owner permissions, a second user with Global Administrator permissions, and set up the required vulnerable workloads:

```powershell
PS C:\> Invoke-WebRequest https://bit.ly/owner-scenarios
-O owner-scenarios.ps1
PS C:\> Get-ChildItem
```

# What does the script do?

The script creates a user account with Global Administrator permissions in Azure AD and another user with Owner permissions in the Azure subscription. See *Figure 7.10* for a diagram:

![](img/4fa338e84e0a590967a142dd798ba4e2_284_0.png)

Figure 7.10 – Global Administrator/Owner exploit scenario

5. After the script has completed, the key information that you will need for the rest of this exercise will be displayed in the output section. Copy the information into a notepad document for later reference. There are four values that you will need from the output section:

**Azure Global Admin User:** This is the administrator username with Global Administrator permissions in Azure AD.

**Azure Global Admin User Password:** This is the password of the Global Administrator user.

**Azure Owner User:** This is the administrator username with Owner permissions to Azure resources.

**Azure Owner User Password:** This is the password of the Azure resource Owner user.

The information can be seen in the following screenshot:

![](img/4fa338e84e0a590967a142dd798ba4e2_285_0.png)

Figure 7.11 – Obtaining the script output

You have now successfully created resources that we will be working with in later exercises in this chapter. In the next hands-on exercise, we will walk through how to exploit the elevate access option to move laterally from Azure AD to Azure resources.

# Hands-on exercise – Elevating access

Here are the tasks that we will complete in this exercise:

- **Task 1:** Verify current user access.
- **Task 2:** Elevate access using the Azure CLI.
- **Task 3:** Verify access to Azure resources.

Let's go through the steps to complete the tasks:

1. Open **PowerShell** on your Pentest VM.
2. In the **PowerShell** console, type `bash` to switch to **Windows Subsystem for Linux**.
3. Use the following command to switch to the root user. Enter the administrative password when prompted:

```
azureuser@PentestVM:~# sudo su -
```

4. Authenticate to Azure using the Azure CLI command – `az login`:

```
root@PentestVM:~# az login
```

Here is a screenshot of this:

![](img/4fa338e84e0a590967a142dd798ba4e2_286_0.png)

Figure 7.12 – az login results

5. When prompted, enter the Azure Global Administrator credentials that you obtained from the script output in the first exercise in this chapter:

![](img/4fa338e84e0a590967a142dd798ba4e2_286_1.png)

Figure 7.13 – Azure password prompt

You will receive an output message about not having access to a subscription. This is because the account only has access to Azure AD, and no access to any Azure subscriptions.

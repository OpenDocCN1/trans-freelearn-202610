
7. Run the following command to exploit the Global Administrator role to modify privileges to Azure resources:

   ```bash
   az rest --method post --url "/providers/Microsoft.Authorization/elevateAccess?api-version=2016-07-01"
   ```

   If you did not receive any output, this means that the command completed successfully.

8. Sign out and then sign in again with the Global Administrator credentials to confirm that you now have permissions on Azure subscription resources:

   ```bash
   az logout
   ```

   ```bash
   az login
   ```

   Here is a screenshot of this:

   ![](img/4fa338e84e0a590967a142dd798ba4e2_288_0.png)

   *Figure 7.16 – AZ CLI login with subscription access*

9. Assign the subscription Owner role to the Global Administrator account using the following commands:

   ```bash
   userPrincipalName=$(az ad signed-in-user show --query userPrincipalName -o tsv)
   az role assignment create --role "Owner" --assignee $userPrincipalName
   ```

The commands will be successful, proving that you have used your elevated Global Administrator role to modify permissions for Azure resources! Here is the output of the command:

```
root@PentestVM:~# userPrincipalName=$(az ad signed-in-user show --query userPrincipalName -o tsv)
root@PentestVM:~# az role assignment create --role "Owner" --assignee $userPrincipalName
{
  "canDelegate": null,
  "condition": null,
  "conditionVersion": null,
  "description": null,
  "id": "/subscriptions/204cce89-.../providers/Microsoft.Authorization/roleAssignments/5ac7db39-887b-40c6-a743-e562f2120a6a",
  "name": "5ac7db39-887b-40c6-a743-e562f2120a6a",
}
```

Figure 7.17 – Owner role assignment

We have now modified our privileges to an Azure subscription from Azure AD. Let's examine the reverse scenario and techniques that can be employed to achieve this.

## Escalating from subscription Owner to Azure AD roles

As part of a pentest, you could also be interested in pivoting from an Azure subscription to Azure AD. This objective could be part of an attack chain that has a goal of opening backdoors in Azure AD for persistence. We will cover the topic of persistence in *Chapter 8, Persisting in Azure Environments*. Here are some techniques that could be leveraged to achieve this.

### Path 1 – Exploiting privileged service principals

Similar to user accounts, service principals and managed identities can also be assigned to Azure AD roles. Many attackers consider service principals and managed identities to be easier targets as they are usually excluded from security policies such as conditional access and MFA.

An attacker could exploit the privileges of an Azure AD account with rights to service principals or managed identities to gain access to the security privileged principals. This is a possible path, but it may be rare for you to run into, especially if an organization strictly follows the principle of keeping role assignments separate for the scopes.

### Path 2 – Exploiting service principals' API permissions

The Microsoft identity platform supports two types of permissions for service principals: delegated permissions and application permissions. Delegated permissions are used for interactive authentication flows, where a signed-in user is present. For this scenario, the user will need to consent to the permission at the time of access (or an administrator can consent on behalf of users).

Application permissions are used by apps that run without any user interaction, which means no user is signed in to them. An administrator only needs to consent to the granted permissions once for the app. It is not uncommon for service principals to be granted permissions to perform sensitive operations on APIs such as the Microsoft Graph. These permissions can be exploited by an attacker to access the APIs that the app has been granted access to.

![](img/4fa338e84e0a590967a142dd798ba4e2_290_0.png)

Figure 7.18 – Service principal app permission

For example, an app may not have any role assignments in Azure AD, but if it has app permissions to perform sensitive operations in Microsoft Graph, this could be used as an indirect way to access Azure AD. This style of attack is also becoming a popular phishing technique for attackers. By using an Azure AD consent grant URL (*Figure 7.19*), an attacker can use a legitimate Azure URL to send to a phishing victim. This URL uses the built-in Azure AD permissions approval flow to allow users to approve external applications.

Since there are many potential ways to escalate in an internal environment, we will focus on some general concepts that we will use to pivot from Azure down to on-premises systems.

## Identifying connections to on-premises networks

First, we will look at how we can identify network connections that bridge the cloud to an on-premises network. These connections can allow us to pivot to other networks that may allow for escalation in the general Active Directory environment.

> **Important note**
>
> Since a lab for this scenario would require you to set up a connection to another network, we will not be covering this in a hands-on exercise. But we will provide the instruction on how to find these connections.

From the Azure Portal, finding on-premises connections is an easy task. Just navigate to the **Connections** blade and a list of available connections will appear. These connections can be **Site-to-site VPN** or **ExpressRoute** connections.

### Site-to-site (IPsec) connections

Site-to-site connections rely on traditional network hardware (VPN concentrators, routers, and so on) to create a VPN tunnel from Azure to another network. Many organizations choose this option, as it allows them to integrate with existing technologies that they have in place. One of the primary differences with site-to-site is that the traffic flows over the public internet, versus a private backbone connection.

### ExpressRoute connections

For those who want a more dedicated (and private) connection to Azure from their network, ExpressRoute connections are the preferred option. These connections are typically created for organizations through a service provider that has a relationship with Microsoft.

While it is good to understand the basics of how these connections work, what we really care about is how we can use these connections to attack other networks. Since a Virtual Gateway will be used to create these connections, we will need to review the settings for the gateway to determine which networks have access to the remote connection.

In general, if we have the Contributor (or Owner) role on a subscription, we should be able to access virtual machine resources in the virtual network with access to the connection. Worst-case scenario, we could always create a new VM in the appropriate virtual network or attach a network interface to an existing VM.

With access to an on-premises network, and some domain credentials, we will want to find any available options for escalating our privileges in the domain. We will cover some common options and tools in the following sections.

## Identifying domain escalation paths

Since we are assuming that we have escalated to an Owner role, or to an account with access to multiple subscriptions, we will assume that some domain credentials were gathered at some point along the way. If not, we would recommend revisiting previous chapters to review ways that we can extract domain credentials from virtual machines and PaaS services.

Using these acquired domain credentials, we will want to use tools to identify resources in the Active Directory domain that can be used to escalate our privileges. In many environments, multiple hops between systems will be required to gain access to domain administrator rights.

As an example, here is a common domain escalation path for an Active Directory domain (`ACME.local`) that is integrated with Azure resources:

- An attacker gathers domain credentials (Password Guessing, Network Protocol Attacks, and so on) from Active Directory or an Azure VM.
- The attacker uses the credentials to get a Kerberos ticket in a crackable format as part of a Kerberoasting attack.
- The ticket is cracked, revealing the cleartext credentials for the associated service account (`ACME\SQLUser1`).
- The service account has local administrator rights on the AD servers computer group.
- A server in the group (`FileServer01`) has an active logon session for the `ACME\kfosaaen-DA` account, a Domain Administrator.
- The attacker uses the SQLUser1 service account credentials to log in to the `FileServer01` server as a local administrator, and dumps the Domain Administrator credentials from the LSASS process with Mimikatz.

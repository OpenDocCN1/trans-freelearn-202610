
By providing a "desired state" of "run my malicious executable" for a virtual machine, we can ensure that the code that we want to run on the VM is run persistently.

> **Important Note**
>
> A technique (using VM DSC extensions versus Automation account DSCs) very similar to the one outlined in this section has been covered in greater detail on the NetSPI blog, in a post written by Jake Karnes: https://www.netspi.com/blog/technical/cloud-penetration-testing/azure-persistence-with-desired-state-configurations/.

For example, we can use the following process to connect a VM to the DSC service and set the *desired state*, which runs a script on the VM:

1. Add the target VM as a DSC node to the Automation account service.
2. Upload a configuration.
3. Compile the configuration.
4. Assign the node configuration to the VM.
5. Wait for the DSC to indicate the node is out of compliance and run the *correction* script.

For the configuration, we have a few options for *desired states*. From a persistence perspective, we would want to have a configuration that allows remote access. The following generalized configuration could be used to ensure that our backdoor is continually run:

- A file (`testfile.exe`) should exist on the C drive at `C:\testfile.exe`.
- A process should be running on the system from a specific path (`C:\testfile.exe`).
- If either condition is not satisfied, correct the condition.

Keep in mind that the DSCs can be a bit of a pain to get working, but they are very well hidden within the Azure interface, so this will help keep the execution under wraps and hopefully give you a decent method for persistence.

Here is one example of what we could do with a Run as account with a privileged AAD role:

1. Assign the Run as account a privileged AAD role (Global Admin, User Admin, and so on).
2. Create a runbook that adds a new user (with a known password) to the AAD tenant.
3. Assign the new user a privileged role in AAD.
4. Use the new user as a short-term channel to access AAD.

Finally, we have multiple options for executing these runbooks to maintain our access. We previously covered using the schedule method, but there is an additional method that can be used to start runbooks. The **webhook** method can be a great way to trigger your persistence runbooks, even when you've lost access to the environment:

![](img/4fa338e84e0a590967a142dd798ba4e2_323_0.png)

Figure 8.22 – Creating a new webhook for an automation account

Webhooks are URLs that can be used to remotely trigger an Automation account runbook. The real benefit of webhooks is the ability to execute them on demand. Where a scheduled runbook execution is helpful, it does require you to wait a set time for a job to be run again. This results in multiple executions of the runbook, which may draw more attention to the persistence method. By using a webhook, you can control when the runbook is run, and you can limit the number of times that it executes.

While Automation accounts are technically PaaS services, they’re so useful for persistence that we’ve separated them into their own subsection. Next, we will cover additional PaaS services that can be used for persistence in an Azure environment.

## Maintaining persistence to PaaS services

Most platform services in Azure require both network and user authorization for access (*Figure 8.23*). For persistence to be effective, it has to be on two levels – network persistence and user persistence:

![](img/4fa338e84e0a590967a142dd798ba4e2_324_0.png)

The most common targets of platform service persistence are data stores or secret stores such as platform SQL databases, storage accounts, Cosmos DB, container registries, and key vaults.

### Maintaining network persistence to platform services

Depending on resource configuration, network access could be over a public endpoint or a private endpoint (*Figure 8.24*). A public endpoint means that the resource can be reached from the public internet, whereas a private endpoint means that the resource can be reached from a private virtual network in Azure.

Persisting over a public endpoint involves modifying the configured authorized IP range so that it includes an IP address. This can be done using any of the Azure management tools or by using offensive tools, such as Lava. For example, the `sql_backdoor_firewall_rule` module in Lava will modify the authorized IP range of a platform SQL server so that it includes a specified IP (Figure 8.24). As you can imagine, this modification could easily draw the suspicion of the security operations team:

![](img/4fa338e84e0a590967a142dd798ba4e2_325_0.png)

A more subtle approach for a service such as Azure SQL may be to modify the **Firewall exception** network setting to allow access to **Azure services and resources** (Figure 8.25). This setting allows access to any Azure IP address, including public IP addresses that belong to other organizations! Note that this behavior is mainly for Azure SQL and not other services with networking restrictions such as storage accounts, key vaults, or container registries. We recommend using this with caution:

![](img/4fa338e84e0a590967a142dd798ba4e2_325_1.png)

Figure 8.25 – Modifying the Azure SQL firewall exception

### Maintaining user persistence to platform services

Depending on resource configuration, user access could be using **key-based access**, such as access keys, **Shared Access Signature (SAS)** tokens, or **identity-based access**, which typically uses Azure AD. We learned how to harvest passwords and access keys using MicroBurst in *Chapter 6, Exploiting Contributor Permissions on PaaS Services*. The majority of platform service access keys can easily be regenerated if they are compromised.

For a service such as Azure Storage, SAS tokens that grant privileged access to storage resources can also be generated for persistence. Using this option can be more difficult to detect for the security operations team as the operation is not explicitly logged in the Activity Log.

To persist using identity-based authorization, RBAC role assignments can be modified to add access to a backdoor user. We will discuss options for persisting in Azure AD in the next section.

## Persisting in an Azure AD tenant

As the core of any Azure environment, the Azure AD tenant is a great target for persistence. Once we have escalated rights within the tenant, we have several options for creating (or modifying existing) security principals that can be used to persist in the environment. In general, creating new principals in an Azure AD tenant may create more alerts than modifying existing resources, but your mileage may vary there.

As a general path for persistence, we will need to gain access to an identity, ensure that the identity has the necessary permissions, and (if needed) create policy exceptions to allow continued access to the identity.

To identify this event, security operations teams can look for the `Add application` or `Add service principal` activity types or operations in the Azure AD audit log (**Azure portal → Azure AD → Audit Logs**).

### Inviting a Guest User Identity into the Tenant

Another way to obtain a backdoor identity for an Azure AD tenant is to invite a guest user account. This can be trickier if the collaboration restrictions setting is set to the most restrictive option, as shown in the following screenshot. This setting can be found in **Azure portal → Azure AD → User settings → Manage external collaboration settings → Collaboration restrictions**:

![](img/4fa338e84e0a590967a142dd798ba4e2_330_0.png)

Figure 8.28 – Verifying the external collaboration settings

If this is the case, attempting to modify this option to add another domain is likely to trigger an alarm, as organizations that configure this option are most likely to be monitoring modifications to the list. Collaboration restriction modification events are logged as `Add policy` operations in the Azure AD audit log.

If the setting has been configured for the most inclusive option, we could invite users from generic domains such as `outlook.com` and `gmail.com` into the tenant. By default, any user or guest user can be used to invite external identities into the tenant (*Figure 8.29*)!

This setting can be found in **Azure portal → Azure AD → User settings → Manage external collaboration settings → Guest invite settings**:

![](img/4fa338e84e0a590967a142dd798ba4e2_330_1.png)

Figure 8.29 – Verifying the guest invitation settings

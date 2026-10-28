

8. 以管理员身份打开一个新的 PowerShell 控制台。将 **步骤 4** 中的值设为变量。将 `LOGIN_SERVER` 和 `ACCESS_TOKEN` 替换为 **步骤 4** 中的值：

   ```powershell
   PS C:\> $loginserver="LOGIN_SERVER"
   PS C:\> $accesstoken="ACCESS_TOKEN"
   ```

9. 要将镜像从注册表拉取到本地缓存，你需要使用 PowerShell 控制台（而不是 WSL）。这是因为 WSL 无法访问本地安装的 Docker 应用程序。Windows 10 的较新版本（build 18362 或更高）支持此功能，但在我们的实验室 VM 上的构建版本中尚未支持。

10. 从 PowerShell 控制台连接到容器注册表：

    ```powershell
    PS C:\> docker login $loginserver -u 00000000-0000-0000-0000-000000000000 -p $accesstoken
    ```

11. 使用以下命令将镜像从容器注册表拉取到本地缓存：

    ```powershell
    PS C:\> docker pull $loginserver/nodeapp-web:v1
    ```

以下是命令及其输出的截图：

![](img/4fa338e84e0a590967a142dd798ba4e2_178_0.png)

我们可以测试此凭据以查看它是否仍然有效以及其拥有的访问级别。请记下 APP_ID、APP_KEY 和 TENANT_ID 值。

14. 切换回你已打开的 WSL 会话。使用以下命令以服务主体身份进行身份验证。将 APP_ID 替换为 *步骤 10* 中的 APP_ID 值。将 SECRET_KEY 替换为 *步骤 10* 中的 APP_KEY 值。将 TENANT_ID 替换为 *步骤 10* 中的 TENANT_ID 值。按 *Enter*：

    ```bash
    root@PentestVM:~# az login --service-principal --username APP_ID --password SECRET_KEY --tenant TENANT_ID
    ```

你现在已作为服务主体登录。接下来，我们将验证此服务主体拥有的权限。

15. 使用以下命令验证服务主体的角色分配和范围。将 APP_ID 替换为 *步骤 10* 中的 APP_ID 值：

    ```bash
    root@PentestVM:~# az role assignment list --assignee APP_ID --include-groups --include-inherited --query '[].{username:principalName, role:roleDefinitionName, usertype:principalType, scope:scope}'
    ```

你可以看到，此服务主体在订阅范围内被分配了 Contributor 角色。太棒了！我们已成功利用从容器注册表中的镜像中搜集到的凭据，将权限从 Reader 角色提升到 Contributor 角色！

![](img/4fa338e84e0a590967a142dd798ba4e2_180_0.png)

在本章的这一点，你可以关闭打开的窗口，因为本章的其余部分将涵盖 Azure 门户中的场景。下一个场景将专门针对具有 Azure AD Reader 权限的用户。需要注意的一点是，下一个场景甚至可能被没有订阅的 Azure AD 用户利用。

## 利用动态组成员身份

本章的最后一部分介绍如何操作 Azure AD 账户详细信息以使账户符合动态组成员身份的条件。目前，我们经常看到动态组用于设备管理和账户的地理分组。随着越来越多的组织将 Active Directory 管理迁移到 Azure AD，这些动态组将变得更加流行。

如 *第 1 章、Azure 平台和架构概述* 中所述，动态组成员身份基于为 Azure AD 账户关联的参数设置的规则。这些规则可能简单到将来自特定城市的所有用户添加到组中。规则可以配置的方式有多种，因此在实际场景中我们可能会看到一些相当有趣的组成员身份逻辑。

对于此示例场景，我们将查看 azurepentesting 租户中的 **Dynamic Admins** 组。此组有一条规则，允许电子邮件地址中包含单词 admin 的任何用户成为该组的成员。该组还为订阅应用了 Owner 角色。以下是动态规则的示例：

![](img/4fa338e84e0a590967a142dd798ba4e2_181_0.png)

图 4.50 – 示例动态组成员身份规则

虽然此 Azure AD 租户的管理员可能认为该组已适度锁定，但他们没有考虑到一个致命弱点。在我们的示例租户中，我们允许访客账户。组织需要一些外部协作选项，因此我们所有用户默认都可以邀请外部访客账户。

这次攻击的优点是，初始用户无需在订阅上拥有任何权限即可执行攻击。他们只需要能够邀请访客用户。虽然查看 IAM 权限很有帮助，但在此情况下的任何用户都可以查看一个动态组（`subscriptionOwners`）并做出一些假设。

作为具有 Azure 门户访问权限的用户，我们可以邀请一个新用户，该用户的电子邮件地址恰好包含 admin：

![](img/4fa338e84e0a590967a142dd798ba4e2_182_0.png)

图 4.51 – 用户邀请屏幕

这会导致向 `admin@fosaaen.com` 账户发送邀请邮件，用户可以接受对租户的访问。

这里需要注意的一个重要步骤是需要使用现有的 Azure 账户作为邀请的收件人。你可以为特定电子邮件地址创建邀请，但该电子邮件需要在另一个租户（Microsoft 或其他）中创建 Azure 用户后，才能正式添加到你的受害租户中。此外，用户需要满足从受害租户下推的任何 MFA 和条件访问要求。

一旦访客账户添加并准备就绪，我们可以看到用户现在已应用 Owner 角色并成为动态组的成员：

![](img/4fa338e84e0a590967a142dd798ba4e2_183_0.png)

图 4.52 – 动态组成员身份示例

从实用性角度来看，我们可能不会看到大量动态组被用来授予 Azure 订阅的权限。然而，我们在咨询项目中见过各种各样的客户端环境，并可以证明在 Azure AD 中实施了更奇怪的配置。

> **重要说明**
>
> 有关此攻击的更多信息，请查看 Cody Burkard 在 Mnemonic 博客上对此技术的原始博文：https://www.mnemonic.no/blog/abusing-dynamic-groups-in-azure/。

至于此问题的缓解措施，我们建议将动态组成员身份规则设置为基于用户无法控制的参数进行操作。此外，锁定访客账户访问权限以防止访客用户操作自己的账户也会有所帮助。

## 实践练习 – 清理 Owner 利用场景

在此最终练习中，我们将使用清理脚本自动删除本章场景中设置的资源。以下是本练习中将完成的任务：

- 从 GitHub 下载清理脚本。
- 运行脚本以删除为场景创建的对象和资源。

以下是完成这些任务的步骤：

1. 打开网络浏览器并浏览到 `https://portal.azure.com` 的 Azure 门户。使用 `azureadmin` 凭据登录。
2. 在 Azure 门户中，单击左上角的 Cloud Shell 图标。选择 **PowerShell**：

![](img/4fa338e84e0a590967a142dd798ba4e2_184_0.png)

图 4.53 – 打开 Cloud Shell

3. 在 Cloud Shell 窗格内的 PowerShell 会话中，运行以下命令下载用于创建具有 Reader 权限的用户账户并设置所需易受攻击的工作负载的脚本。还验证下载：

   ```powershell
   PS C:\> Invoke-WebRequest http://bit.ly/reader-scenario-cleanup -O reader-scenario-cleanup.ps1
   PS C:\> Get-ChildItem
   ```

以下是其外观的截图：

![](img/4fa338e84e0a590967a142dd798ba4e2_185_0.png)

图 4.54 – 下载脚本的结果

4. 运行以下命令运行下载的脚本以删除本章练习中创建的对象和资源：

   ```powershell
   PS C:\> ./reader-scenario-cleanup.ps1
   ```

等待脚本完成。完成可能需要大约 8 分钟：

![](img/4fa338e84e0a590967a142dd798ba4e2_185_1.png)

图 4.55 – 运行清理脚本的结果


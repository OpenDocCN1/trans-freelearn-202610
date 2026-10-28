# 3 安装、配置与维护

现在，你应该已经很好地理解了CiviCRM和支持者关系管理工具能为你所在组织做些什么。你也应该花时间规划你的实施工作，包括思考现有数据源以及如何将它们映射到CiviCRM中。不要低估规划过程的重要性——尽管你可能急于安装CiviCRM并开始使用它，但如果你在前期规划上投入更多时间，你的项目将更有可能成功。

在本章中，我们将：

- 安装CiviCRM
- 逐步完成系统的初步配置
- 讨论你需要了解的进阶配置选项，包括Drupal下的访问控制权限
- 回顾升级过程
- 概述推荐的维护流程
- 规划备份策略

由于CiviCRM可以在Joomla!或Drupal内容管理系统（CMS）中实施，本章某些内容将针对每种环境而有所不同。我们已尽量明确指出哪些指导是特定于环境的。在阅读本章时，请务必注意与你实施相关的上下文。

本章的某些部分可能更具技术性，因为我们提供了基本的故障排除步骤并讨论了托管环境注意事项；不要被技术讨论吓倒。对于大多数实施来说，安装和配置过程是快速且轻松的。同样值得注意的是，CiviCRM的安装、配置和升级是使用CiviCRM过程中最技术性的部分。如果需要，你可以将部分或全部工作外包给专业服务，例如列在 http://civicrm.org/professional 和 http://civicrm.org/moreprofessionals 上的服务。

### 安装CiviCRM

> 以下说明假设你已经有了可正常运行的Joomla!或Drupal安装。如果你尚未设置好CMS环境，应在继续本节之前完成设置。

首先访问CiviCRM网站 http://civicrm.org/download ，或直接访问 http://sourceforge.net/projects/civicrm/files/ 选择要安装的CiviCRM版本。我们建议始终在生产站点使用稳定版本。对于Joomla!安装以及Drupal的浏览器/FTP安装方法，你需要将安装包下载到本地机器，然后上传到服务器。针对Drupal讨论的高级命令行安装方法允许你直接下载包到服务器，从而跳过此步骤。

在实施新的CiviCRM安装时，请务必始终访问下载站点，因为该项目会定期发布小版本修订，你需要确保使用最新（且无缺陷）的代码库。

根据你的环境，CiviCRM有三种可用的安装包：

- Drupal
- Joomla!
- Joomla!-alt

CiviCRM 2.0-3.1版本提供了独立的CiviCRM版本，可以在CMS之外运行，或可能集成到其他CMS中。独立版本的支持在v3.2中被移除。如果你使用的不是Joomla!/Drupal内容管理系统，或有其他原因需要研究独立实施，请考虑联系核心开发团队以确定独立支持的最新状态。许多不使用Joomla!/Drupal环境的实施者发现，创建一个用于托管CiviCRM的Joomla!/Drupal“外壳”站点，然后将各种前端页面和表单集成到他们的主网站中更容易。

Joomla!-alt包与标准Joomla!包的不同之处在于它不包含嵌入式压缩文件。如果你检查标准Joomla!安装包，会发现它包含一系列安装文件和一个大的.zip包。相比之下，解压-alt版本会显示所有应用程序文件（即主包内没有子包）。

那么你应该使用哪一个？标准包之所以如此构建，是为了改善Joomla!扩展安装器中的处理；一般来说，你会希望使用那个包。如果安装遇到任何困难，请尝试使用替代包。具体来说，如果你收到一条错误消息，提示PHP缺少.zip功能，则需要使用-alt版本。

除了上面列出的三个版本外，你还会看到语言包文件（-l10n）和可用于验证包完整性的MD5哈希文件（MD5SUMS）。

CiviCRM具有强大的多语言支持。语言包包含当前CiviCRM版本支持的所有语言的翻译文件。

请注意，其中一些翻译并不完整，在投入真实环境之前应彻底审查和测试。通常，面向公众用户页面中的字符串会首先被翻译，管理字符串和较少使用功能的翻译则相对滞后。

在某些情况下，你可能会发现可以合并不同地区的现有语言资源文件以扩展其覆盖范围。例如，加拿大法语和法国法语的独立翻译工作被合并到一个新的加拿大法语文件中，当两个语言文件中都有可用翻译时，该文件使用加拿大术语。这使得“State/Province”被渲染为“Province”而不是“Département”。

有关CiviCRM翻译和本地化工作的更多详情，请访问：

- 本地化指南：http://wiki.civicrm.org/confluence/display/CRMDOC/CiviCRM+Localisation
- 在线翻译工具（Transifex）：http://translations.civicrm.org
- 本地化论坛：http://forum.civicrm.org/index.php/board,10.0.html

在撰写本书时，CiviCRM提供以下语言的翻译（可能仅为部分翻译）：

南非荷兰语、阿尔巴尼亚语、保加利亚语、加泰罗尼亚语、中文（中国-简体）、中文（台湾-繁体）、捷克语、丹麦语、荷兰语、英语（澳大利亚）、英语（英国）、爱沙尼亚语、芬兰语、法语、法语（加拿大）、德语、希腊语、希伯来语、印地语、匈牙利语、印尼语、意大利语、日语、高棉语、挪威语、波兰语、葡萄牙语、葡萄牙语（巴西）、罗马尼亚语、俄语、斯洛伐克语、斯洛文尼亚语、卡斯蒂利亚西班牙语、墨西哥西班牙语、瑞典语、泰卢固语、泰语、土耳其语和越南语。

还有针对阿拉伯语、波斯尼亚语、加泰罗尼亚语、英语（加拿大）、挪威博克马尔语、波斯语、泰米尔语和乌克兰语的额外翻译团队。这通常意味着该语言在早期版本中已有翻译，这可能为新翻译提供一些有用的材料。如果你对多语言支持感兴趣，请考虑使用前述工具参与翻译。

> 在继续安装之前，请确保你的托管环境满足前一章列出的最低要求。尽管你可能在不满足这些基本技术要求的环境中成功安装CiviCRM，但如果这些要求未得到满足，你可能会发现某些功能无法正常工作。

### 在Joomla!中安装

CiviCRM是使用Joomla!的扩展工具**安装/卸载**安装的，该工具可从Joomla!网站管理界面的**扩展**菜单中获取。然而，与大多数Joomla!扩展不同，你将无法使用**上传包文件**选项安装该软件。这是为什么呢？这仅仅是因为CiviCRM是一款非常庞大的软件——大小是Joomla!本身的两倍多，而且大多数托管环境并未配置为在单个进程中容纳上传包并运行安装脚本所需的资源（如**浏览并安装**选项那样）。因此，我们必须使用Joomla!可用的替代工具。





请仔细遵循安装说明，因为它们与许多其他 Joomla! 扩展的安装流程不同。

按以下步骤在 Joomla! 中安装 CiviCRM：

- 1. 使用 FTP 软件将安装包上传到您的网站。我们建议您将包上传到 /YOUR_JOOMLA_ROOT/tmp/。该文件夹由 Joomla! 创建，是存放安装包最合适的位置。
- 2. 解压安装包。您可以使用命令行或托管在服务器上的基于 Web 的文件浏览器（如 cPanel 文件管理器）来完成此操作。Joomla! 中有各种扩展可让您在 Joomla! 内操作文件系统，这也可能有所帮助。如果您在网上解压安装包时遇到困难，您可能需要在本地计算机上解压它，然后使用 FTP 软件将解压后的文件集上传到 /YOUR_JOOMLA_ROOT/tmp/ 目录。上传并解压后，您应该在 /YOUR_JOOMLA_ROOT/tmp/ 目录内看到一个 /com_civicrm/ 文件夹。
- 3. 在 Joomla! 管理界面中浏览至 Extensions | Install/Uninstall。使用 Install from Directory 选项，输入解压后的 /com_civicrm/ 目录的完整文件位置。在大多数情况下，Joomla! 会预先填充该字段为 /YOUR_JOOMLA_ROOT/tmp/ 目录的正确位置，您只需添加 com_civicrm 即可。

![](img/76a9a15ee6002f74a917dd7fbdfc8b1c_91_0.png)

- 4. 单击 **Install** 后，您应被引导至确认安装成功的页面。单击提供的链接进入 CiviCRM 并确认其已安装并正常工作。

如果您在安装 CiviCRM 时遇到问题，请查阅 *Installation troubleshooting* 部分，以对可能的服务器和环境相关问题进行总体回顾。对于 Joomla! 安装，最常见的问题源于对我们刚刚描述的 **Install from Directory** 方法不熟悉。大多数 Joomla! 用户习惯于更常见的 **Browse and Install** 方法来安装扩展。

需要记住的两个关键事项如下：

- CiviCRM 安装包必须在服务器上解压。您应引用文件夹位置——而不是从 www.civicrm.org 下载的 `.zip` 文件。
- 文件夹位置必须是完整的系统路径——而不是相对于您 Web 目录的路径。例如，它可能看起来像 `/home/user/public_html/tmp/com_civicrm/`。如果您收到以下错误消息：**Please enter a package directory**，则是因为您未引用解压后安装文件的正确路径。

如果您在安装过程中仍继续遇到问题，请访问专门用于 Joomla! 安装的 CiviCRM 论坛：`http://forum.civicrm.org/index.php/board,7.0.html`。

## Installation in Drupal

CiviCRM 使用自动安装程序来配置软件以在 Drupal 中使用。以下说明涉及在现有 Drupal 安装中设置 CiviCRM 实现的过程。

Drupal 支持使用单个代码库来运行多个网站（请参阅 `http://drupal.org/getting-started/6/install/multi-site`）。站点特定数据存储在 `/path_to_docroot/sites/` 下的站点特定目录中，每个站点的域名通常用作其子目录的名称。此站点特定信息包括数据库名称，每个站点唯一，以及存储在 `/path_to_docroot/sites/yoursite.org/settings.php` 文件中的其他连接信息。CiviCRM 在安装过程中对这种多站点共享代码库和独立数据库的方法提供透明支持。在开始安装过程之前，请确保为每个站点创建单独的 CiviCRM 数据库。

尽管这种多站点方法仍然非常流行，但也有一些相反的趋势倾向于单站点安装。在尝试同时升级共享代码库的多个站点时，会出现依赖、复杂性和时序问题。更重要的是，Drush (http://drupal.org/project/drush) 和 Aegir (http://groups.drupal.org/hostmaster2) 已使使用多个相同代码文件副本管理多个单站点安装的单个代码库变得越来越容易。

以下各节中的 CiviCRM 安装程序适用于您的 Drupal 站点是单站点还是我们刚刚描述的多站点配置。如果您想使用高级技术在不同的 Drupal 站点之间共享数据库表以提供单点登录功能（访问 http://thedrupalblog.com/setting-multi-site-drupal-6-installation-shared-databases-and-single-sign）或在多个 CiviCRM 站点之间共享数据库以在站点之间共享联系人和/或管理员（访问 http://wiki.civicrm.org/confluence/display/CRMDOC/Multi+Site+Installation），则需要不同的程序。

根据您的技能和舒适程度，您可以选择以下两种方式之一安装 CiviCRM。第一种使用浏览器和 FTP 客户端应用程序。第二种使用 SSH，即安全命令行 shell，对于具备基本 Linux 技能的人来说可能更快、更方便。

### Browser/FTP procedure

在开始安装过程之前，请注意您 Drupal 安装的数据库设置。如果您不知道这些值，可以在 /path_to_docroot/sites/default/settings.php（默认位置）找到它们。

找到 $db_url 变量：

```
$db_url = 'mysqli://dbuser:dbpassword@localhost/drupal_dbname';
```

这里：

- Database server: localhost
- Drupal database name: drupal_dbname
- Database user name: dbuser
- Database user password: dbpassword

较旧的站点可能仍在使用 mysql 驱动程序，而不是 mysqli：

```
$db_url = 'mysql://dbuser:dbpassword@localhost/drupal';
```

如果您的站点包含此项，您应 definitely 考虑升级到 mysqli。mysqli 最初引入是为了支持 MySQL 4.1 功能，现在在 PHP 5.x 中非常稳定且得到良好支持，并且相对于 mysql 驱动程序提供了性能改进。只需将 $db_url 中的驱动程序规范从 mysql 更改为 mysqli，Drupal 即可获得这些好处。

您现在可以开始安装过程。按照以下步骤操作：

- 1. 如果您尚未这样做，请从 http://sourceforge.net/projects/civicrm/files/（使用 Download Now! 按钮是确保您获得正确 tarball 的最佳方式）下载最新稳定版的 Drupal CiviCRM 安装 tarball 到您的本地机器。
- 2. 将 CiviCRM 安装包从本地机器上传到 Drupal 安装中的 /path_to_docroot/sites/all/modules/ 目录。将 CiviCRM 和其他模块安装在此目录下将使文件与核心 Drupal 模块分离，并促进任何未来的 Drupal 核心升级。（如果您不希望 CiviCRM 可用于运行此代码库的其他站点，您可以在站点目录下创建 modules 子目录，例如 /path_to_docroot/sites/default 或 /path_to_docroot/sites/yourdomain.org，并在那里安装它。）如果需要，调整 /path_to_docroot/sites/ 目录的权限，以为运行 Web 服务器进程的帐户提供写入权限（完成后将其恢复为原始权限）。这通常可以使用您的 FTP 或 SFTP 客户端应用程序完成，也可以使用以下命令：

  ```
  $ cd /path_to_docroot/sites/ ; chmod -R 666 ./*
  ```

- 3. 解压安装包。您现在应该在站点中拥有以下目录：/path_to_docroot/sites/all/modules/civicrm/，其中包含其他子目录。
- 4. 使用适当的工具（如 phpMyAdmin 或您的托管面板）为 CiviCRM 创建单独的 MySQL 数据库。如果您在下一步中输入一个在数据库服务器上尚不存在的数据库名称，并且您输入的数据库用户具有创建数据库的权限，安装程序将自动为 CiviCRM 创建新数据库。我们不建议授予不需要该权限的数据库帐户（如用于 Drupal 站点的帐户）创建数据库的权限。





> 通常认为，出于备份和升级的目的，最好将 Drupal 和 CiviCRM 放在不同的数据库中。可以考虑使用以域名作为数据库用户名的命名约定，并在其后附加 `dru` 和 `civ`，以分别表示 Drupal 和 CiviCRM 数据库。对于同一域名的暂存站点，你可以将数据库命名为 `domainname_staging_dru` 和 `domainname_staging_civ`。这样做的好处是，在创建数据库用户时，可以一次性轻松地为所有这些数据库设置权限。
>
> 请注意，如果你打算使用 Drupal Views 模块在 Drupal 页面中显示 CiviCRM 数据，并且打算为 Drupal 和 CiviCRM 使用不同的数据库用户，则需要确保你的 Drupal 数据库用户对 CiviCRM 数据库具有 SELECT 权限。按照上述说明，为 Drupal 和 CiviCRM 使用同一个数据库用户即可满足这一要求。

5. 下一步是运行安装程序，它将确认你已下载正确版本的 CiviCRM，并确保你的服务器满足软件要求。安装程序会在你的数据库中创建必要的表，并使用所需的变量配置 `civicrm.settings.php` 文件。要运行安装程序，请以具有管理员权限的用户身份登录 Drupal，并将浏览器指向以下 URL：`http://yourdomain.org/sites/all/modules/civicrm/install/index.php`。

6. 这将打开 CiviCRM 安装工具。使用你之前记下的 Drupal 数据库详细信息以及你为 CiviCRM 数据库创建的信息填写表单，然后点击重新检查配置和要求。如果有任何系统设置不满足要求，你会收到通知，并有机会解决这些问题。返回此页面并重新检查，直到所有问题都得到解决。你还可以选择安装示例数据（这有助于快速了解系统）以及选择备选语言（如果你已获取翻译文件并将其上传到 CiviCRM 目录）。

7. 单击**检查要求并安装 CiviCRM**按钮以继续安装。如果安装脚本成功完成，你将看到绿色进度条和成功通知。在安装过程中，CiviCRM 为匿名用户和已认证用户配置了访问 CiviCRM 工具和表单的标准权限。你需要检查这些设置，以确认它们符合你网站的需求。我们将在本书后面讨论与 CiviCRM 相关的 Drupal 角色和权限。

> 请注意，在安装过程中，Drupal 会在 /path_to_docroot/sites/ 子目录下为你的站点创建一个文件目录，并使其可写。默认情况下，CiviCRM 将在此目录下为临时文件创建文件夹，例如 /path_to_docroot/sites/yoursite.org/files/civicrm。

安装脚本应自动启用 CiviCRM 模块，并在你的菜单中显示 CiviCRM 链接。单击 CiviCRM 菜单项进入，确认安装成功。

如果你以管理员身份登录后在主菜单中看不到 CiviCRM 链接，请浏览到“管理 | 站点构建 | 模块”，确认 CiviCRM 模块已启用。如果模块已启用但仍看不到 CiviCRM 链接，请访问专门讨论在 Drupal 下安装 CiviCRM 的论坛以进一步排查问题：http://forum.civicrm.org/index.php/board,6.0.html。

## Drush 过程

Drush（Drupal shell）是一个 Linux shell 工具，可用于对你的 Drupal 安装执行操作，包括多站点管理。在 2010 年，社区开始努力通过 Drush 为 CiviCRM 提供支持。在使用之前，你必须先安装 Drush 和 CiviCRM 支持文件（详见下文）。使用 Drush 需要 PHP 5.2+。截至 CiviCRM 3.2 版本，Drush CiviCRM 命令如下：

| 命令 | 描述 |
|---|---|
| `civicrm-cache-clear` | 清除整个 CiviCRM 数据库和 Smarty 缓存。 |
| `civicrm-install` | 安装一个新的 CiviCRM 实例。 |
| `civicrm-rest` | 用于访问 CiviCRM API 的 REST 接口。它可以返回 XML 或 JSON 格式的数据。 |
| `civicrm-restore` | 从指定的备份目录恢复 CiviCRM 代码库和数据库。 |
| `civicrm-sql-cli` | 使用 CiviCRM 的凭据打开 SQL 命令行界面。 |
| `civicrm-sql-conf` | 打印 CiviCRM 数据库连接详细信息。 |
| `civicrm-sql-connect` | 连接 CiviCRM 数据库的字符串。 |
| `civicrm-sql-dump` | 使用 mysqldump 将 CiviCRM 数据库导出为 SQL。 |
| `civicrm-sql-query` | 对 CiviCRM 数据库执行查询。 |
| `civicrm-update-cfg` | 更新 config_backend 以修正配置设置，尤其是在 CiviCRM 站点被克隆/迁移后。 |
| `civicrm-upgrade` | 用新指定的 tar 文件替换 CiviCRM 代码库，并通过执行 CiviCRM 升级过程 `civicrm/upgrade?reset=1` 升级数据库。 |
| `civicrm-upgrade-db` | 从命令行执行 `civicrm/upgrade?reset=1` 过程。 |

如果你尚未安装 Drush，请按以下步骤操作：

1. 将当前版本的 Drush 下载到服务器上的主目录。
2. 对于初学者，最简单的方法是在 http://drupal.org/project/drush 页面点击推荐版本的下载链接下载到本地计算机，然后使用 SCP、SFTP 或 FTP 上传到服务器上的主目录。
3. 对于更高级的用户，一个更简单的方法是将鼠标悬停在 http://drupal.org/project/drush 页面推荐版本的下载链接上，记下下载 URL，然后在 SSH 命令中使用该 URL：
   ```
   $ cd ~; wget http://ftp.drupal.org/files/projects/drush-6.x-3.3.tar.gz
   ```
4. 解压文件：
   ```
   $ tar -xzf drush-6.x-3.3.tar.gz
   ```
5. 重命名 drush 目录：
   ```
   $ mv drush .drush
   ```
6. 使解压后的 drush 目录中的 drush 文件可执行：
   ```
   $ chmod u+x ~/.drush/drush
   ```
7. 确保该目录在你的路径中。
8. 检查以下命令的输出，确保 /usr/local/bin 或某个其他可用目录位于你的 `$PATH` 中：
   ```
   $ set
   ```
9. 在你的路径中的某处（如 /usr/local/bin）创建一个指向 drush 可执行文件的符号链接：
   ```
   $ ln -s ~/.drush/drush /usr/local/bin/drush
   ```
10. 检查 Drush 是否正常工作：
    - 切换到站点的文档根目录：
      `$ cd /path_to_docroot/`
    - 执行 Drush：
      `$ drush -l http://mysite.org help`

现在，按如下方式为 CiviCRM 安装 Drush 支持：

1. 在 CiviCRM 的 Subversion 代码库中找到最新版本的 Drush / CiviCRM 包含文件：在浏览器中导航到 http://svn.civicrm.org/tools/branches/，并记下最新版本，例如 v3.2。
2. 在命令 shell 中，将命令中的 v3.2 替换为你刚刚记下的最新版本，然后执行以下命令：
   ```
   $ cd ~/.drush/includes
   $ wget http://svn.civicrm.org/tools/branches/v3.2/drupal/drush/civicrm.drush.inc
   ```
   现在 Drush 已安装并带有 CiviCRM 支持，安装 CiviCRM 就很容易了。
3. 如果目录尚不存在，请在 `/path_to_docroot/sites/all/modules`（推荐）或此站点的目录中（例如 `/path_to_docroot/sites/yourdomain.org/modules`）为 Drupal 模块创建一个目录，以防止 CiviCRM 被运行在此代码库上的其他站点使用：
   ```
   $ mkdir -p /path_to_docroot/sites/all/modules
   ```
4. 通过在浏览器中导航到 http://sourceforge.net/projects/civicrm/files/ 找到要安装的 tar 包，并选择正确的版本来替换以下命令中的 3.2.0（注意版本号在 URL 中出现两次）：
   ```
   $ wget -O /path_to_docroot/sites/all/modules/civicrm.tar.gz http://sourceforge.net/projects/civicrm/files/civicrm-stable/3.2.0/civicrm-3.2.0-drupal.tar.gz/download
   ```
5. 现在，使用以下命令通过 Drush 安装 CiviCRM，并将 yourDbUser、localhost、yourCivicrmDbName 和 3.2.0 替换为适当的值：
   ```
   $ drush civicrm-install --dbuser=yourDbUser --dbpass=yourDbPassword --dbhost=localhost --dbname=yourCivicrmDbName --tarfile=sites/all/modules/civicrm-3.2.0-drupal.tar.gz --destination=sites/all/modules
   ```





- 6. 在浏览器中以 Drupal 管理员身份登录到您的网站。
- 7. 浏览至 **Administer | Site building | Modules** 以确认 CiviCRM 模块已启用。
- 8. 您现在应该在菜单中看到 CiviCRM 的链接。点击 CiviCRM 菜单项以进入并确认安装成功。

如果在 CiviCRM 模块启用时仍未在主菜单上看到 CiviCRM 链接，请访问 CiviCRM 专门用于在 Drupal 下安装的论坛：http://forum.civicrm.org/index.php/board,6.0.html。

## 安装故障排除

大多数安装问题都是由于未满足托管服务器要求、托管环境资源不足或对上述安装步骤理解有误造成的。让我们逐一回顾这些情况。

如前所述，CiviCRM 对托管环境有特定的要求，其中包括所使用的 Apache/MySQL/PHP 版本、MySQL 的 InnoDB 支持，以及各种功能所需的特定 PHP 扩展。在某些情况下，如果您在未满足基本要求的情况下尝试安装软件，CiviCRM 会显示错误并提供指导。然而，有时您可能能够在没有描述性错误的情况下安装 CiviCRM，并且可能要到后来才会注意到问题。请务必在安装前查看并确认符合服务器要求。

如果您不确定托管环境运行的是哪个版本的 Apache/MySQL/PHP，请在文档根目录中创建一个名为 `phpinfo.php` 的文件，其中仅包含 `<?php phpinfo(); ?>`。确保其可被您的 Web 服务器读取，然后导航至 `http://yoursite.org/phpinfo.php`。在 Joomla! 中，可以通过 **Help | System Info | PHP Information** 页面查看此信息，无需单独的 `phpinfo` 文件。创建并查看 `phpinfo.php` 文件后，请将其从 Web 服务器上删除，因为它通过暴露系统详细信息而带来一定的安全风险。

第二个常见的安装问题原因是托管资源不足。CiviCRM 是一个资源消耗相对较大的应用程序，您在系统中的安装和操作能力将直接与 Web 主机上可用的资源相关。因此，我们强烈建议在虚拟专用服务器或专用托管上运行 CiviCRM 安装。虽然可能在共享托管环境中运行 CiviCRM，但它更容易出现资源相关问题。

**Domain Information** 用于配置组织的名称、**FROM Email Address**、邮寄地址和其他联系人详细信息。此处的设置很重要，因为它们将被用作从 CiviCRM 发送的系统生成邮件和 CiviMail 邮件的默认发件人地址。请注意，用于发送邮件的电子邮件地址需要在您的托管服务器上是一个有效用户，以便 CiviCRM 能够正常发送邮件（此要求取决于您的托管环境配置）。

如果您计划使用 CiviMail 从网站进行群发邮件，请务必填写域名地址字段，以便其包含在批量邮件中。这是对任何合法群发邮件者的良好做法，并且在某些司法管辖区（例如美国的 CAN-SPAM 法案）以及 CiviMail 本身中是强制要求的。

## 查看和编辑联系人

下一个配置部分 **Viewing and Editing Contacts** 处理与联系人相关的设置，包括联系人记录页面、搜索设置、联系人类型和地址设置。我们将逐一介绍这些区域。

**Site Preferences** 控制系统不同部分中可见的内容区域。例如，如果您不打算将标签用作分类工具，则应将其从视图中移除。是否不需要立即访问变更日志（记录联系人记录何时被编辑、由谁编辑以及时间戳的运行列表）？将其从视图中移除。

首选项分为四个区域：

- **Viewing Contacts**：控制联系人记录顶部出现的标签。
- **Editing Contacts**：控制联系人编辑视图中会出现哪些元素（可从联系人摘要标签访问）。
- **Contact Search**：此高级搜索工具按类型对字段进行分组。您展开组面板以访问其各自的字段。在此处使用选项来禁用您不想显示的面板。
- **Contact Dashboard**：CiviCRM 有一个特殊页面，可向已登录网站用户公开，称为 **Contact Dashboard**。仪表板汇总了与用户相关的记录，包括其会员历史、活动注册历史、捐赠历史和认捐历史。此外，用户还可以查看他们有权查看的相关联系人。

在您最初配置此部分时，可能不确定要选择哪些选项。请记住，您可以随时返回稍后修改设置。随着您逐步实施网站并开始在系统中工作，您会发现此处列出的某些元素是否需要。

此页面的最后一个选项让您设置首选的 **WYSIWYG editor**。编辑器工具栏将出现在网站中的任何 HTML 文本区域。我们建议尽可能使用与您的 CMS 相同的编辑器。对于 Joomla! 用户，未来的 CiviCRM 版本将提供集成系统默认和用户指定的 WYSIWYG 编辑器的支持（从 v3.3 开始）。

**Address Settings** 提供类似的功能，允许您选择用于邮件标签生成和联系人地址块的内容。

对此页面的常见更改包括从邮件标签中移除 **{contact.supplemental_address_2}**（大多数组织使用两行地址），以及隐藏 **Addt'l Address 2**、**Latitude** 和 **Longitude** 值。通常，您将依赖地理编码功能在无需用户参与的情况下填充这些值。

如果您有兴趣生成按地址排序的报告，例如用于步行拉票的名单，那么您应该打开 **Street Address Parsing**。这启用了一个功能，尝试自动解析输入系统中的街道地址的街道号码、街道名称和单元号码。从版本 3.3 开始，支持美国、加拿大英语和加拿大法语街道地址。

此页上的最后一个设置处理地址标准化。CiviCRM 将集成 USPS 网络工具 API 以验证输入的地址。请注意，该工具仅检查地址是否有效；它不会确定联系人姓名是否在该地址有记录。在完成本节并启用该工具之前，您必须向 USPS 申请并获得 ID 和 URL。这可以在以下位置完成：http://www.usps.com/webtools/address.htm。

我们清单中的下一个配置项目是 **Mapping and Geocoding**。CiviCRM 通过 Google 和 Yahoo! 支持地址的自动地理编码。首先，访问您首选的服务并获取密钥。然后，在此页面上选择提供商并输入您的密钥。CiviCRM 将在地址创建或编辑时对其进行地理编码。在查看单个联系人记录或搜索结果列表时，您可以查看地址或多个地址的地图（如果您正在查看搜索结果），同时利用 Google/Yahoo! 提供的其他功能，例如绘制到该位置的路线。

密钥可以从以下位置获取：

- Google: http://code.google.com/apis/maps/signup.html
- Yahoo!: http://api.search.yahoo.com/webservices/register_application

继续，**Search Settings** 控制哪些字段可通过简单搜索工具进行搜索，以及搜索功能如何运行和显示结果。选项包括：

- **Automatic Wildcard**：这使用户能够输入部分文本并返回包含该文本的任何匹配的搜索结果，假设在搜索字符串之前和之后使用通配符。较大的数据库可能希望禁用此选项，因为它会减慢搜索性能并返回更大的结果集。
- **Include Email**：这将同时搜索联系人姓名和电子邮件地址以查找搜索文本。
- **Include Nickname**：这将搜索联系人姓名和昵称以查找搜索文本。如果您大量使用昵称，这是一个非常有价值的功能。如果不是，您可能考虑禁用它，因为如果您有大量联系人，它会减慢搜索处理速度。
- **Include Alphabetical Pager** 和 **Include Order By Clause**：这些是有用的功能，可促进搜索结果的导航。然而，它们都可能影响大型数据库的性能。如果您在搜索时遇到响应时间差，请考虑关闭其中一个或两个。
- **Default Contact Search Profile**：进行搜索后，结果列表表显示联系人姓名、地址、电话和电子邮件。如果您希望设置默认显示的替代字段集，您必须先构建一个配置文件（字段集合），然后从 **Default Contact Search Profile** 选项中选择它。
- **Smart group cache timeout**：这控制智能组中的联系人记录何时从缓存中清除的频率。智能组是保存的搜索；缓存可提高从搜索中提取记录列表时的性能。默认情况下，每次添加或编辑联系人记录时都会清除缓存。这可确保对记录的任何更改都反映在动态搜索结果中。每次清除时重新创建缓存需要时间，尤其是如果您在智能组中有大量联系人。如果您不需要智能组实时准确，可以通过增加此值来提高联系人保存时的页面加载时间（建议五分钟）。
- **Autocomplete Contact Search**：在水平导航栏左上方有一个快速搜索字段。当您在字段中输入时，系统会显示可能与输入值匹配的结果。可能匹配的显示始终包括联系人姓名，但也可能包括电子邮件、电话和地址字段。使用 **Autocomplete Contact Search** 选项选择应包含哪些字段。添加选项可能有所帮助，因为您可以根据电子邮件或其他值区分同名联系人。然而，不要选择太多选项，因为它会使结果下拉列表变得杂乱，难以阅读列表。





- **杂项**：我们清单中的下一页配置页，**杂项**，包含各种系统操作选项。
- **仪表盘缓存超时**：仪表盘是您首次进入系统时显示的 CiviCRM 主页。您可以配置仪表盘以显示报表，或“小部件”，在一个页面上提供系统数据的有帮助摘要。由于小部件报表可能根据您的配置从系统中几乎任何数据中提取，因此页面加载可能需要一些时间。系统缓存通过对报表数据进行静态快照，并在定期基础上才从实时数据重新加载来改善这一点。您可以使用 **仪表盘缓存超时** 选项控制缓存清空的频率。
- **联系人回收站与恢复**：对于需要更严格控制联系人记录删除（包括其捐赠信息）的组织而言，这是一个有用的选项。它可以被禁用以简化工作流程并提高性能。启用后，通过联系人记录、搜索结果任务或合并结果删除的联系人将被“放入回收站”——标记为已删除但保留在数据库中。您以后可以使用高级搜索找到这些联系人并永久删除或恢复它们。
- **版本检查与统计报告**：这有助于您作为实施者跟踪 CiviCRM 的最新版本。如果您安装的版本早于当前稳定版本，CiviCRM 将在管理页面的顶部放置通知以提醒您。如果此选项打开，基本统计信息将被发送回 CiviCRM，以便核心开发团队对人们使用的是什么版本、在什么环境中托管以及数据库的大小有一个基本概念。这些统计信息不包含您网站的任何数据——它们仅包括您的数据和系统环境信息的汇总统计信息。在可能的情况下，请保持此选项开启，因为这对核心团队了解社区如何使用软件非常有帮助；这通常会指导未来开发的方向。
- **最大附件数**：在生成电子邮件或创建活动时，用户可以附加文件。在此处设置 **最大附件数** 以防止一次附加太多。
- **reCAPTCHA**：最后，您可以使用 reCAPTCHA 为在线表单启用反垃圾邮件工具。reCAPTCHA 有助于确保表单由人类而非计算机完成。它以图形形式呈现单词或字母数字字符串，通常带有某种扭曲或其他视觉障碍，使计算机难以破译。如果您计划在网站上提供公开表单供用户输入数据，您应考虑实施此功能。

首先，在 http://recaptcha.net/ 注册以获取公钥和私钥。然后返回此页面完成表单。配置完成后，您可以在个人资料的 **设置** 页面 **高级设置** 面板中按表单逐个启用 **reCAPTCHA**。我们在介绍个人资料表单和向网站公开数据的过程时将进一步讨论此问题。

- **联系人类型**：本节的最后一页配置页是 **联系人类型**。CiviCRM 有三种主要联系人类型——**组织**、**个人** 和 **家庭**。联系人类型用于区分您维护的联系人记录类型。每种类型将具有一些独特的数据字段，以及所有类型共有的字段。例如，名字和姓氏字段适用于个人记录，但不适用于组织记录，例如企业。相比之下，地址字段是通用的。

除了三种主要联系人类型之外，您还可以创建额外的子类型来对记录进行分类。您还可以使用此配置工具重命名现有的主要类型。思考联系人子类型时，有几件重要的事情需要记住：

- 使用子类型组织记录有几个好处，其中最主要的是能够创建分配给一个或多个子类型的自定义字段。考虑一所使用 CiviCRM 跟踪家长和教师的学校。这些都是个人联系人类型，但为每种类型收集的数据会非常不同。学校可能希望了解希望志愿服务的家长的技能，或跟踪父母是否也是该校校友；另一方面，教师记录会跟踪其教育经历、授课历史以及与就业相关的 人力资源数据。创建子类型允许您对收集的数据进行细分。
- 子类型是基于三种主要联系人类型之一的。在创建子类型时，首先考虑子类型应基于哪三种主要类型之一。
- 联系人记录目前只能分配一种联系人类型。因此，请确保您的子类型是互斥的。换句话说，不要创建子类型，使得新联系人可能同时适合多个子类型，或对联系人最佳放置位置存在歧义。还有许多其他方式可以对联系人进行分类；子类型应仅用于高层分类，其中联系人的性质存在根本差异。

有关联系人子类型的更完整回顾以及如何思考系统结构的额外指导，请参阅第 4 章，*CiviCRM 基础：浏览系统和处理联系人*。

## 发送电子邮件

在解决了核心配置和联系人相关设置后，我们现在将注意力转向系统生成的电子邮件。CiviCRM 可以配置为自动为终端用户操作生成电子邮件，例如注册活动或进行捐赠。此外，管理员可以使用活动或 CiviMail（批量电子邮件分发工具）向一个或多个联系人发送电子邮件。虽然您不使用电子邮件发送功能也可以使用 CiviCRM，但如果您选择不使用，您将错过一项重要的功能。

首先，在 **Administer | Configure | Global Settings | Outbound Email (SMTP/Sendmail)** 设置您的 **外发电子邮件** 配置，然后选择基于 **mail()**、**sendmail** 或 **SMTP** 的邮件机制。**SMTP** 可能为退回邮件处理提供更完整的退回头信息，尽管默认的 **mail()** 选项通常已足够。您的托管提供商也可能有偏好，或仅支持某些选项。无论您选择并配置哪个选项，请务必使用内置测试功能并确认收到电子邮件。这可以快速轻松地确保外发电子邮件正常工作。

> 如果您计划使用 CiviCRM 进行大规模群发电子邮件，请注意维护与 ISP 以及维护白名单/黑名单的组织良好的发送者声誉所涉及的责任。还要注意大规模邮件发送可能造成的潜在服务器负载。如果您计划大规模使用 CiviMail，您可能需要考虑第三方 SMTP 提供商。

在测试外发电子邮件后，在 **Administer | Configure | From Email Addresses** 配置设置。在从 CiviCRM 发送电子邮件时，登录用户的电子邮件将是默认的发件人地址。但是，您可以配置系统可用的额外地址，并在发送电子邮件时选择它们。

例如，您的组织可能有基于服务的通用电子邮件，例如 `events@my.org`、`questions@my.org` 和 `membership@my.org`。根据您的托管设置，所使用的发件人电子邮件地址可能需要在您的邮件服务器中作为邮箱或转发存在。

## 处理退回电子邮件流量

CiviMail 组件为处理传入电子邮件提供了重要功能。这些电子邮件由系统接收和处理，目的有三：



## 维护良好的电子邮件服务器声誉

系统发送的任何群发电子邮件都将被互联网上运行的各种垃圾邮件检测系统监控。虽然你无法直接控制这些系统，但你有能力也有责任构建和管理你的群发电子邮件系统，以最大程度地降低被标记为垃圾邮件发送者的可能性。这一声誉管理流程涉及域名记录配置，包括在电子邮件中提供发件人详细信息，提供退订和选择退出工具，以及处理退回的电子邮件，以便你不会反复向错误的电子邮件地址发送邮件，等等。所有这些都有助于降低垃圾邮件评分，并确保你能够通过群发电子邮件有效地继续触达你的受众。在本节中，我们将查看与 DNS（域名记录）相关的注意事项。

第一步是在你的域名的 DNS 中设置 SPF 记录，并在移动服务器时确保其正确配置。发件人策略框架（SPF）向世界标识出被明确允许从你的域名发送电子邮件的主机、不允许的主机，以及无法对其做出判断的主机。你的 CiviCRM 服务器和域名的外发邮件服务器（如果不同），都应在 SPF 中被标识为“允许发送（通过）”。你的 SPF 记录越具体、越有限，它在有助于减少 CiviMail 投递问题方面的效果就越好。不幸的是，携带笔记本电脑在不同 IP 之间旅行的用户在发送电子邮件时，可能会使锁定发件人策略框架变得困难。

改善服务器声誉的第二种有用方法是设置反向 DNS。在《发送电子邮件》部分，你应该已经配置并成功测试了外发电子邮件设置。为了确定一切是否真的正常工作，请使用 CiviCRM 向你可以在服务器外部访问的账户发送一封电子邮件，例如 Gmail 或 Hotmail 账户。然后，检查电子邮件头中的几项内容。在 Gmail 和 Google Apps 中，你可以这样做：

1. 打开该消息。
2. 点击 Reply 或 Reply to all 旁边的其他操作下拉菜单。
3. 选择 显示原文。

查找 Received-SPF 头记录。它应指示 neutral 或 pass。你也可能会找到其他记录，例如 Authentication-Results: spf=neutral。

如果你在 SPF 上遇到问题，问题有时可以追溯到服务器在发送电子邮件时提供的身份。检查 Received: From 头记录，直到找到你的源服务器，也许还要追溯到你的域名的 localhost。将你遇到的任何问题或疑问向你的 ISP 的技术支持提出。

反向 DNS 是将 IP 地址翻译为主机名的过程，与正向 DNS 过程相反，正向 DNS 将常用的域名翻译为 IP 地址。设置反向 DNS 记录（如果尚未设置）可以帮助改善你的电子邮件声誉，因为发送 IP 被验证为你域名的合法发件人。如果你的服务器有多个域名共享一个 IP，那么花少量资金购买一个专用 IP（对 CiviContribute 的 SSL 证书也很有用）可能是值得的，以便你可以为你的域名设置反向 DNS。共享 IP 环境可能会导致你的域名评分不佳，如果它与垃圾邮件发送者或其他在电子邮件声誉管理方面不够谨慎的账户位于同一 IP 段中。

改善服务器声誉的第三种方法是监控 DNS 黑名单，并请求维护这些列表的组织在你的服务器出现在列表中时将其移除。你可能因各种原因被列入名单，无论是正当的还是不正当的，包括：你没有正确处理退信，你的电子邮件收件人可能在投诉时出现错误，或其他网站影响了你的声誉。及时响应黑名单非常重要，因为它通常会从一个服务传播到另一个服务。

## 配置电子邮件处理器

要处理传入电子邮件，你必须有一个电子邮件账户来接收传入电子邮件，并配置一个 cron 作业定期处理收到的电子邮件（本章后面会介绍）。

虽然不是必需的，但你通常希望用于退信处理的电子邮件账户专门用于此目的。你通常也不希望它是一个公开的账户，供受众向其发送电子邮件。它应仅为退信处理目的而创建和配置。

CiviMail 使用可变信封返回路径（VERP）处理作为跟踪退信的方法。该方法为每封外发消息添加一个唯一的发件人地址。当退信返回时，此唯一发件人地址用于确定哪条消息、哪个电子邮件地址和联系人记录产生了退信。退信按类型分类（在可能的情况下），并根据系统中的规则进行处理。不同的退信类型会触发对电子邮件的临时或永久保留，或记录交付尝试以供将来处理，如果该地址有额外的退信。

VERP 的首选设置称为子地址，它使用本地部分格式。如果你的隐形退信处理电子邮件地址是 `civimail@example.org`，那么 VERP 地址会在分隔符之后、域名之前附加一个唯一值，例如 `civimail+12.234ka.241123@example.org`。虽然这是首选方法，但一些 ISP 故意在其服务器上阻止子地址，因为它在群发邮件中被广泛使用。

为了测试你是否可以使用此方法，尝试向你设置的账户发送一封电子邮件，在 @ 符号之前向账户附加 +test 或 -test（myemail+test@mydomain.org）。如果成功收到消息，则确认支持本地部分处理，并且可以准备设置账户以处理传入消息。

如果不支持子地址/本地部分处理或你不希望使用该方法，你仍然可以使用 catch-all 电子邮件账户来处理退信电子邮件。Catch-all 账户配置为接收未发送到你域上设置的其它邮件账户的所有电子邮件——它“捕获”未被其它邮件账户“捕获”的任何电子邮件。一般来说，catch-all 账户不太理想，因为它们通常会“捕获”大量垃圾邮件，这些邮件可能会与合法的退信一起处理。要确定你的托管环境是否支持 catch-all 账户，请登录管理你的电子邮件账户的系统并查找指定电子邮件为 catch-all 的选项，或联系托管支持以确定系统功能。如果你的系统支持 catch-all 地址但不支持子地址，你的服务器负载会略高，但系统应该能正常运行。

许多 CiviCRM 安装运行在虚拟专用服务器上。在大多数情况下，VPS 的管理员有权根据自己的需求和偏好配置其电子邮件系统。如果你自己没有技术技能，值得提出支持请求来按你想要的方式设置。

如果你的电子邮件系统无法同时支持子地址或 catch-all 地址，支持 VERP 的最后一种方法是使用第三方账户，例如 Gmail。处理远程电子邮件存储的处理时间和资源负载更高，如果发件人域名与 FROM 电子邮件域名不同，你的声誉可能会受到轻微影响；但这仍然是一个可行且有效的解决方案。请注意，根据所使用的系统，你可能需要配置过滤器，以便退信不会被标记并归档为垃圾邮件。例如，Gmail 账户应创建过滤器，将任何被标记为垃圾邮件的内容重新定向回收件箱，以便 CiviCRM 处理。

按如下方式使用你设置的退信处理账户配置 CiviMail：

1. 点击 **Administer | CiviMail | Mail Accounts**。
2. 点击默认账户行的 **Edit** 链接。
3. 按需更改 **Name**，并将 **Return-Path** 留空。
4. 输入该账户的 **Server**、**Username**、**Password** 和 **Email Domain**。
   - 你应该能够使用这些设置配置 Web 或桌面电子邮件客户端来检索邮件。





5. 如果账户支持子地址，请在“Localpart”字段中输入电子邮件地址“name+”（包含加号，例如“civimail+”）。

6. 如果可能，请在**Protocol**字段中配置系统使用 IMAP（优于 POP3 或其他选项）。
   - 再次，使用你的电子邮件客户端测试账户是否正常工作，然后再期望 CiviMail 能与其通信。

7. 如果可能，请设置 SSL 以提高安全性。本地颁发的证书应该可以。

配置好账户后，通过在浏览器地址栏输入以下 URL 手动检查邮件处理器脚本是否正常工作，其中 ?name=<username>&pass=<password>&key=<sitekey> 中的值与下面“设置 Cron 作业”部分中解释的值相同。例如：
- Drupal: `http://example.org/sites/all/modules/civicrm/bin/EmailProcessor.php?name=<username>&pass=<password>&key=<sitekey>`
- Joomla!: `http://example.org/administrator/components/com_civicrm/civicrm/bin/EmailProcessor.php?name=<username>&pass=<password>&key=<sitekey>`

你应该在浏览器窗口中收到一条消息，说明账户已成功访问以及是否处理了任何电子邮件。通过在 CiviCRM 中创建一条联系人记录并故意使用错误的电子邮件地址继续测试。将电子邮件添加到邮件列表组，通过 CiviMail 发送测试电子邮件，然后再次运行脚本。这次，你应该会看到一条消息，说明已处理一封电子邮件，如果你查看联系人记录，你应该会看到该电子邮件被置于保留状态。

在确认退信处理功能正常工作后，**不要忘记配置 cron 作业**，如本章后面所述，以自动化常规处理功能。

请注意，上面概述的配置指导涵盖了退信处理的标准设置选项。还有其他配置选项可用，例如 return-path 头和替代协议选项，如果你的服务器设置是非标准的，则可以使用这些选项。有关这些选项如何使用的更多详细信息，请查看在线文档或在论坛中搜索指导。

## 在线支付处理器

你决定实施 CiviCRM 的原因之一很可能是为了受益于通过网站处理付款并立即在联系人管理系统中记录的能力。无论你是在收集捐款、竞选捐款、赞助承诺、认捐、会员费，甚至是注册费，所有这些交易都将通过 CiviContribute 进行。

然而，在你可以通过 CiviCRM 收集和处理付款之前，必须配置一个支付处理器。你的支付处理器，也称为支付网关，是你购买的一项服务，允许直接通过你的网站或通过重定向到处理器的网站进行信用卡处理。处理器接收信用卡，授权付款，并将资金转入你的银行（手动或自动，具体取决于服务）。

选择要使用哪些服务可能具有挑战性，因为有许多选项可用。你需要确保你选择的任何服务都有相应的 CiviCRM 支付处理器插件可用；否则，你可能需要开发一个新的插件来与你选择的服务集成。

在撰写本书时，以下插件可用：
- Authorize.net AIM
- Click and Pledge
- DPS Payment Express
- Elavon Payment Processor
- eWAY (Single Currency)
- FirstData (Linkpoint)
- Google Checkout
- Moneris
- Payflow Pro
- PayJunction
- PayPal – Express
- PayPal – Website Payments Pro
- PayPal – Website Payments Standard
- Realex Payment

一些额外的处理器，包括 Chase Paymentech 和 Quickbooks，已经创建或部分创建，但由于某种原因，它们尚未完全集成或打包到核心软件中。

并非所有支付处理器都一样。有些仅在有限的地区可用，或对可用货币有限制。收取费用的结构和模式也会有很大差异。如果你计划使用定期捐款，请确保调查处理器是否允许此类功能，并确保它已构建到 CiviCRM 插件中。

> 要获取提供其功能详细信息的受支持处理器的最新列表，请访问 http://wiki.civicrm.org/confluence/display/CRMDOC/CiviContribute+Payment+Processor+Configuration。请注意，并非所有处理器插件都得到 CiviCRM 项目的官方支持。有些是由社区贡献的，并依赖社区支持进行持续更新。

## 集成与重定向处理器

有两种基本的支付处理器类型，即允许你直接从网站收集和处理信用卡（集成）的处理器，以及将你重定向到处理器的网站来处理交易（重定向）的处理器。两者各有利弊：
- 允许直接从网站收集信用卡的处理器提供了更具吸引力和品牌化的解决方案。用户永远不会离开你的网站，交易流程从收集表单无缝流向确认页面，最后到感谢/收据页面。然而，处理服务收取的费用通常更高，你必须拥有专用 IP 并安装 SSL 才能充分保护交易。
- 将你重定向到处理网站的处理器（例如 PayPal Standard 和 Google Checkout）不需要 SSL（因为安全性由处理网站处理），并且通常可以非常快速地实施，并且可能收取较低的总费用。然而，重定向到处理网站意味着访客必须离开你的网站才能完成交易。这可能会阻止或使人们对完成付款感到困惑。

> CiviCRM 不会与共享 SSL 配合使用（通常在许多共享托管环境中提供）。你必须在你的域名上安装 SSL 才能使 CiviCRM 正常工作。如果你为你的网站实施 SSL，请务必访问：Administer | Configure | Global Settings | Resource URLs 并启用 Force Secure URLs (SSL) 选项。这将把捐款页面和 CiviCRM 管理页面重定向到 SSL。

如果你是网上支付的新手，并且不确定你的支持者会对新功能做出何种反应，请考虑从离站解决方案开始，在该解决方案中你会被重定向到处理器的网站来完成交易。在某个时候，你可能会达到一个阈值，即直接通过你的网站处理交易的额外成本是合理的。

SSL 证书是特定于域名的。为 www.yoursite.com 购买的证书将不会验证 yoursite.com，因为域名的根形式和 www 形式不被视为相同。
此外，CiviCRM 通过站点 URL 引用一些资源，如果根形式和 www 形式被互换使用，可能无法一致地运行。最简单的方法是通过在公共网站目录中的 .htaccess 文件中添加 Apache 指令来解决。例如，如果你希望从根形式重定向到 www 形式，请添加以下内容（使用你的域名）：

```
RewriteEngine On
# Redirect root domain to www. form
RewriteCond %{HTTP_HOST} ^yoursite\.com$ [NC]
RewriteRule ^(.*)$ http://www.yoursite.com/$1 [R=301,L]
```

然后，确保你在 CiviCRM 配置中对资源的引用都使用域名的 www 形式。除了确保 CiviCRM 和你的 SSL 证书正常运行之外，这还有助于创建你网站的一致域名品牌。

## 配置支付处理器

一旦你决定使用哪个支付处理器并完成了与你使用的公司必要的申请流程，你可以在 CiviCRM 中设置账户。从配置清单中选择 Payment Processors。

你必须填写的字段取决于你使用的处理器。在几乎所有情况下，CiviCRM 都允许你为实时处理器和沙箱（测试）账户配置凭据。当你使用捐款页面中的测试驱动选项时，将使用沙箱凭据。有关创建沙箱账户的详细信息，请参阅你的支付处理器。

如果你尚未拥有支付处理器账户，但希望开始设置和测试捐款页面或其他支付处理表单，请使用 Dummy Payment Processor 选项。然后你可以配置你的表单并启用捐款处理（包括活动和会员表单）。使用虚拟处理器时不会触发任何交易——它应仅用于临时测试目的。

CiviCRM 支持配置多个处理器。在你创建捐款页面或活动注册页面时，将要求你选择要由此表单使用的所需处理器。





配置好支付处理器后，务必通过生成一条真实的捐助记录来测试它。不要仅依赖沙盒/测试工具；在使用会向公众处理付款的捐赠/承诺/活动/会员表单之前，务必使用真实信用卡运行一次真实交易来测试系统。

## 系统工作流模板

完成支付处理器的设置后，返回配置清单并访问 **System Workflow Templates** 页面。当人们访问您网站上的 CiviCRM 表单，以及您作为管理员处理联系人记录时，系统会生成各种电子邮件并发送给您的联系人。所有这些电子邮件都可通过此页面进行自定义。

如果您编辑其中任何一封电子邮件并保存更改，您将有选项查看或恢复到原始版本。

![](img/76a9a15ee6002f74a917dd7fbdfc8b1c_117_0.png)

模板包含占位符令牌，用于联系人和记录特定字段值。例如，如果您希望电子邮件以“Dear First Name”开头，您将插入 **First Name** 令牌。在编辑模板文本时，您可以使用 **Insert Tokens** 按钮插入基本联系人字段。

编辑模板时请谨慎。大多数模板都包含许多条件子句（`if/else` 语句），以确保生成的电子邮件针对特定交易进行定制。进行编辑后，请务必使用各种选项进行测试，以确认其正常运行。

> CiviCRM 使用 Smarty 模板引擎（http://www.smarty.net）来渲染页面，包括工作流电子邮件。在创建电子邮件模板时，您可以插入基本逻辑并使用其他 Smarty 函数。

## 组织、自定义和组件

此时，您已完成了初始站点配置中最为关键的步骤。配置清单中包含我们将在此处简要回顾，并在后续详细介绍其各自功能完整实现时进一步讲解的额外部分。这些额外部分为 Organize your contacts、Customize Data、Forms and Screens，以及 Components。

### Organize your contacts

当您开始导入和录入联系人时，您会开始寻找组织和分类他们的方法。CiviCRM 中有多种方法可以做到这一点，但两种主要方法是使用标签和组。标签是您创建的分层分类树，用于对联系人进行分类，或是一份自由文本的运行列表（tagset）。组是记录集合，可以是动态创建的（Smart Groups，相当于保存的搜索）或手动收集的（例如， opt-in 邮件列表）。我们将在第 4 章 CiviCRM Basics: Moving through the System and Working with Contacts 中讨论这些的配置和实现。

### Customize Data, Forms and Screens

CiviCRM 的核心数据字段涵盖了您所期望的所有基本联系人和通信字段。不可避免地，您将拥有特定于您组织的额外数据片段，希望将其记录在数据库中。CiviCRM 的 Custom Data 工具允许您向几乎任何现有类型的记录附加额外字段。例如，您可以向联系人记录、捐助记录、活动注册、关系类型等添加字段。我们将在第 5 章 Collecting, Organizing, and Importing Data 中查看创建自定义字段。

在许多，但并非所有情况下，您创建的数据字段都将包含在在线表单中并向网站访客公开。我们使用 Profiles 来选择核心和自定义字段以包含在特定表单中。我们将在第 6 章 Communicating Better 中开始了解如何构建和使用配置文件。

### Components

如本章前面所述，CiviCRM 组件代表软件的主要功能区域。您的首次配置步骤是启用/禁用计划使用的组件。配置清单中的最后一个区块可让您访问每个组件特定的设置和选项。当我们在本書中探索每个功能区域时，我们将检查每个组件特定的配置选项。

## 选项列表

虽然我们已完成了配置清单的回顾，但仍有几个额外的设置值得我们探索。您可以选择现在配置这些区域，或简单地记下它们，并在实施过程中需要时再返回修改。

在使用 CiviCRM 的过程中，您会很快发现有许多选项列表为字段提供选择。这些包括位置类型、电话类型、个人前缀和后缀、活动类型等等。可配置选项的完整列表可在 Administration Console 上找到，最常修改的部分列表可在 Administer | Option Lists 中获得，如以下屏幕截图所示：

![](img/76a9a15ee6002f74a917dd7fbdfc8b1c_119_0.png)

每个选项的可用设置将特定于选项类型。在大多数情况下它是不言自明的；大多数都附带内联帮助文本。如果选项同时包含名称和标签字段，则名称字段用于唯一选项值，标签字段是选项列表中显示的内容。

## 与 CMS 用户同步

如果您的 Joomla! 或 Drupal 网站已运行一段时间，您可能已有现有的网站用户账户列表。除非用户已登录网站并访问过 CiviCRM 页面，否则这些用户账户不会立即拥有对应的 CiviCRM 联系人记录。作为初始系统配置的一部分，最好将这些 CMS 用户与 CiviCRM 同步。访问此选项的方法是访问 Administer | Manage | Synchronize Users to Contacts。

触发此工具将为网站中的每个 CMS 用户创建关联的联系人记录。通常只需启动一次同步即可。此后，新 CMS 用户将在创建或访问 CiviCRM 资源时与 CiviCRM 关联。

> 这将以多种方式发生，具体取决于您使用的是 Joomla! 还是 Drupal，以及您实施了哪些账户创建工具。我们将在本书后面回顾一些可用选项。

让我们花点时间准确了解 CiviCRM 联系人与 CMS 用户之间的关系。

您的网站（CMS）用户是拥有使用用户名和密码登录网站权限的个人。CiviCRM 联系人是驻留在您捐助者数据库中的记录。CiviCRM 在 CMS 用户与其对应的 CiviCRM 联系人记录之间创建链接。当用户登录您的网站时，您就可以公开该联系人特定的数据字段。例如，您可以让已登录的联系人编辑其地址和电话详细信息。

虽然每个 CMS 用户都应有一个对应的 CiviCRM 联系人记录，但反之通常不成立。您通常会维护没有对应用户账户的个人（以及组织和家庭）的联系人记录。

在配置和规划站点时，开始思考谁应该有权访问站点；您可能希望向网站用户公开哪些数据字段以供编辑和更新，以及您希望为创建和授予新用户权限采用的工作流程。

## Drupal 对 CiviCRM 的访问控制

Drupal 具有内置于其核心的有效的细粒度权限系统，可通过特定模块以多种方式扩展。CiviCRM 扩展了 Drupal 访问控制系统，以允许控制 CiviCRM 内的特定功能。我们将在本节中假设对 Drupal 访问控制系统有基本了解（访问 `http://drupal.org/node/22275`）。

> CiviCRM 目前不支持 Joomla! v1.5 的访问控制权限，主要由于 CMS 的访问组/级别限制。随着 Joomla! v1.6 的发布，其中包含强大的权限系统，CiviCRM v4.0 版本将包含完整的访问控制集成。下面找到的 Drupal 权限列表将与 Joomla 1.6 兼容版本中可用的权限相同。

访问是基于用户角色提供的，并且可以根据个人承担或放弃对 CiviCRM 中功能和内容的访问和管理的责任而添加或删除。





Drupal 站点始终具有两个内置角色：匿名用户（任何未登录的人，包括没有用户账户的人），以及已认证用户（当前已登录的任何人）。通常还会为系统管理员以及需要被允许执行他人不被允许执行的操作的人群创建额外的角色，例如审核评论或其他用户生成内容。

我们不建议为 CiviCRM 创建过于复杂的角色集。理想情况下，您将设置权限，使人们始终能够执行他们需要执行的操作，并且不能执行对系统、其数据或组织保护其持有的个人隐私声誉有害的操作。通常您只需使用一个名为 CiviCRM 管理员的角色即可，或许再加一个允许用户查看和编辑财务信息的角色。

以下是可在 Drupal 中通过 管理 | 用户管理 | 权限 按角色选择性启用或禁用的权限：

- access CiviCRM
- access CiviContribute
- access CiviEvent
- access CiviMail
- access CiviMail subscribe/unsubscribe pages
- access CiviMember
- access CiviPledge
- access CiviReport
- access Contact Dashboard
- access Report Criteria
- access all custom data
- access deleted contacts
- access uploaded files
- add contacts
- administer CiviCRM
- administer Reports
- administer Tagsets
- administer dedupe rules
- administer reserved tags
- delete activities
- delete contacts
- delete in CiviContribute
- delete in CiviEvent
- delete in CiviMail
- delete in CiviMember
- delete in CiviPledge
- edit all contacts
- edit all events
- edit contributions
- edit event participants
- edit groups
- edit memberships
- edit pledges
- import contacts
- make online contributions
- merge duplicate contacts
- profile create
- profile edit
- profile listings
- profile listings and forms
- profile view
- register for events
- translate CiviCRM
- view all activities
- view all contacts
- view event info
- view event participants

正如您所见，这些权限可分为不同类型的组：

- 访问 CiviCRM 的组件（例如，访问 CiviMember）
- 对特定类型的对象执行某些操作（例如，添加联系人）
- 访问管理界面（例如，管理重复数据删除规则）
- 访问非管理功能（例如，网上捐款和注册活动）

Drupal 权限是累积的——如果用户被分配了三个角色，则来自所有三个角色的组合权限将决定用户在系统中的操作权限。由于权限明确允许指定的访问，如果三个角色中的任何一个已被授予权限，它将影响用户的访问。

CiviCRM 中包含三个 Drupal 模块，允许 CiviCRM 中的联系人信息以与 Drupal 访问控制系统有用交互的方式使用。

## CiviGroup Roles Sync

此模块允许将特定 Drupal 角色中的所有用户放置到 CiviCRM 组中，反之亦然。这允许 CiviCRM 将联系人放入组中，并使该结果在 Drupal 中产生额外的访问权限。CiviCRM 个人资料表单上的注册可以自动添加到组中，然后可用于启用对您网站部分的访问。还可以为所有捐赠者或其他标准（如地理位置）创建智能组，以提供对您网站部分的差异化访问。

要启用 CiviGroup Roles Sync，请按以下步骤操作：

- 1. 在 Drupal 中，点击 **Administer | Site building | Modules**。
- 2. 在 **CiviCRM modules** 字段集中，点击启用 **CiviGroup Roles Sync** 模块。
- 3. 点击 **Save Configuration** 按钮。

要配置 CiviGroup Roles Sync，请按以下步骤操作：

- 1. 创建一个或多个非默认 Drupal 角色，如下所示：
  - 在 Drupal 中，点击 **Administer | User management | Roles**。
  - 输入新角色的名称，然后点击 **Add role** 按钮。
  - 将一个或多个用户分配给新角色，例如：
  - 在 Drupal 中，点击 **User management | Users**。
  - 点击用户名旁边的 **edit**。
  - 在 **Roles** 下，启用新角色旁边的复选框。
  - 点击 **Save**。
- 2. 创建一个或多个 CiviCRM 角色，如下所示：
  - 在 CiviCRM 中，点击 **Administer | Manage | Access Control**。
  - 点击 **Manage ACL Roles**。
  - 点击 **Add ACL Role**。
  - 在 **Label** 字段中输入角色名称。您还可以选择性地输入 **Description**，增加或降低角色的 **Weight** 以使其在相对于其他角色的列表中显示得更低或更高，或取消选中 **Enabled?** 字段以禁用该角色。
  - 点击 **Save**。
- 3. 在 **Contacts | Manage Groups** 下根据需要创建一组用户以分配给 CiviCRM 角色。

> 访问控制必须在组的设置中启用为组类型。

- 4. 将访问控制组中的用户分配给每个 CiviCRM 角色：
  - 点击 **Administer | Manage | Access Control**。
  - 点击 **Assign Users** 以将用户分配给 CiviCRM ACL 角色。
  - 点击 **Add Role Assignment**。
  - 在 **Assigned To** 中选择一个 ACL 角色和一组用户。
  - 可选择取消选中 **Enabled?** 以禁用该分配。
  - 点击 **Save**。
- 5. 在 Drupal 中，点击 **Administer | Site configuration | CiviGroup Roles Sync**。
- 6. 点击 **Add Association** Rule。
- 7. 从选择列表中选择一个 CiviCRM 组和一个 Drupal 角色。
- 8. 点击 **Add association** rule。

## CiviMember Roles Sync

此模块在特定会员类型与特定会员状态和 Drupal 角色之间提供双向同步。例如，这允许您为所有当前会员提供对站点的访问，并在他们未支付会员续费时自动移除该访问。

要启用 CiviMember Roles Sync 模块，请按以下步骤操作：

- 1. 在 Drupal 中，点击 **Administer | Site building | Modules**。
- 2. 在 **CiviCRM** 模块字段集中，点击启用 **CiviMember Roles Sync** 模块。
- 3. 点击 **Save Configuration** 按钮。

要配置 CiviMember Roles Sync，请按以下步骤操作：

- 1. 创建一个或多个非默认 Drupal 角色，如下所示：
  - 在 Drupal 中，点击 **Administer | User management | Roles**。
  - 输入新角色的 **Name**，然后点击 **Add role** 按钮。
  - 将一个或多个用户分配给新角色，例如：
  - 在 Drupal 中，点击 **User management | Users**。
  - 点击 **Username** 旁边的 **edit**。
  - 在 **Roles** 下，启用新角色旁边的复选框。
  - 点击 **Save**。
- 2. 在 **Administer | CiviMember | Membership Types** 和 **Administer | CiviMember | Membership Status Rules** 处创建和配置 CiviCRM 会员类型和状态（有关更多详细信息，请参阅 *Chapter 8, Growing Your Membership and Interacting with Members*）。
- 3. 在 Drupal 中，点击 **Administer | Site Configuration | CiviMember Roles Sync**。
- 4. 点击 **Add Association Rule** 选项卡。
- 5. 选择一个 CiviMember 会员类型并选择一个 Drupal 角色，以便为具有该会员类型当前状态的每个联系人分配该 Drupal 角色。
- 6. 为 **Current Status** 和 **Expired Status** 启用适当的身份验证状态。如果您使用的是默认的 CiviCRM 会员状态规则，您可能希望为 **Current Status** 启用 **New**、**Current** 和 **Grace**，为 **Expired Status** 启用 **Expired**、**Pending**、**Cancelled** 和 **Deceased**。当联系人的会员类型更改时，将检查这些列表以确定是否应将所选 Drupal 角色分配给或从联系人的 Drupal 账户中取消分配。
- 7. 点击 **Add association rule**。

## CiviCRM OG Sync

此模块可确保 CiviCRM 组中联系人的更改反映在关联的 Drupal 组织组中。然而，由于组织组模块没有提供适当的方式通知 CiviCRM 组织组成员身份的更改，我们发现此模块的实用性有限。





## Drupal 下的 CiviCRM 访问控制

除了上面我们看到的 CiviCRM 暴露给 Drupal 访问控制权限系统的权限之外，CiviCRM 还有自己的访问控制功能，提供更细粒度的控制。一般来说，你可以通过 CiviCRM 的界面控制更多类型的操作。此外，你还可以指定接收权限的数据子集；例如，你可以限制一组用户对组成员联系人字段的编辑权限。

> Joomla! 1.5 的限制使得 CiviCRM 在安装在 Joomla! 下时无法提供自己的访问控制。CiviCRM v4.0 将支持 Joomla 1.6，并提供这些功能。

为了简化在 Drupal 和 CiviCRM 中配置你的访问控制系统，你可能想要考虑使用 CiviGroup Roles Sync 模块。这样，相同的用户就可以同时拥有相同的 CiviCRM 角色，就像用于 Drupal 访问控制的 Drupal 角色一样。当你采用这种方法时，给 CiviCRM 和 Drupal 角色使用相同的名称是非常好的做法。

可以分配给角色的 Drupal 权限往往结合了两个概念——操作和数据集。例如，“访问评论”包括“访问”操作和“评论”作为操作对象的数据。CiviCRM 访问控制中的权限将这些概念形式化并分开。因此，它们包括以下三个元素，而不是 Drupal 的两个：

- CiviCRM 角色
- 操作（查看、编辑、创建、删除、搜索、全部）
- 数据

尽管这使得指定权限变得有点繁琐，但它提供了一个非常灵活的结构来控制访问。

在前一节关于 CiviGroup Roles Sync 和 CiviMember Roles Sync 的讨论中，我们回顾了如何创建 CiviCRM 访问控制角色（在 管理员 | 管理 | 访问控制，管理角色）以及如何将用户分配给这些角色（在 管理员 | 管理 | 访问控制，将用户分配给 CiviCRM ACL 角色）。现在是时候把所有东西整合起来并创建 CiviCRM 访问控制列表了：

1. 点击 管理员 | 管理 | 访问控制。
2. 点击 管理 ACL。
3. 点击 添加 ACL。

## 仪表盘

当你首次登录你的 CMS 并访问 CiviCRM 时，你会进入仪表盘页面。此页面可以配置为以两列显示来自系统的报告。它可直接从此页面使用 配置你的仪表盘 按钮进行配置。

点击后会显示两列的视觉表示以及可用报告的列表。只需从可用报告块中拖放至两列之一，然后点击 完成 以保存你的设置。

可用报告，或称小部件，来自主报告列表（报告菜单）。你必须先将报告配置为可作为小部件使用，然后它才会出现在仪表盘配置页面中。

## 导航

在我们对 CiviCRM 的回顾中，我们引用了工具和选项在水平导航栏中的默认位置。然而，你可以添加、移动、删除或重命名站点中的菜单选项。例如，如果你的组织是活动（用于跟踪与支持者的沟通）的重度用户，你可能希望将 活动搜索 和 新活动 菜单项移至主标题。

你可以通过前往 管理员 | 自定义 | 导航菜单 来修改导航菜单。菜单以可展开的树形显示。点击 新菜单项 按钮创建新选项，右键点击现有选项以修改它，并拖放项目以将其移动到新位置。

在创建或编辑选项时，你输入页面的 URL。站点内的页面可以使用相对 URL 引用。然而，你也可以使用完整 URL（包括 http://）创建指向非 CiviCRM 位置和站点外部链接的菜单选项。Drupal 站点将使用 权限 选择工具来确定哪些用户可以查看和访问菜单项。

对菜单进行修改后，页面顶部会出现一个通知块。点击 点击此处 链接重新加载菜单并查看你的更改。

## 设置 cron 作业

cron 作业是配置在你的托管服务器上的基于时间的触发器。你设置 cron 作业以定期自动运行脚本。CiviCRM 附带了几个可选脚本，可通过 cron 运行，提供各种功能的自动化批量记录处理。

- `UpdateMembershipRecord.php`：此脚本由 CiviMember 使用，用于根据系统中定义的规则更新会员状态并启动续费提醒邮件。如果你是 CiviMember 用户，这是一个非常重要的脚本。会员状态规则定义了会员记录何时被视为新、当前、宽限期或过期（或你定义的任何其他状态）。必须运行此脚本才能根据这些规则处理记录，并在它们从一个状态转换到下一个状态时更新它们。每天运行一次就足够了，最好在低流量时段（例如午夜）运行，因为它可能是一个资源密集型脚本，具体取决于数据库的大小。如果你使用续费提醒邮件功能（在会员到期日前固定天数向当前会员发送提醒邮件），你必须为会员类型创建邮件模板以供使用。这将在本书后面更详细地讨论。
- `UpdatePledgeRecord.php`：此脚本由 CiviPledge 使用，用于处理捐赠承诺提醒并更新捐赠承诺状态。当用户向你的组织承诺捐赠时，他们确定其承诺的总价值和捐赠频率。如果已配置，此脚本将在下一次捐赠日期临近时发送捐赠提醒邮件。它还会更新捐赠承诺的状态。每天运行此脚本一次就足够了。
- `UpdateAddress.php`：CiviCRM 的地理编码功能会在地址创建或编辑时生成纬度和经度值。如果你有尚未地理编码的现有地址，或最近导入了新的地址记录，请使用此脚本批量生成地理编码值。你可以根据需要从浏览器运行此脚本（例如在地址导入后）。如果你定期导入记录，将脚本配置为按周期（例如每周）运行的 cron 作业可能很有用，或根据需要更频繁地运行。请注意，Google 和 Yahoo 都对每天可通过 API 地理编码的地址数量设置了限制。当数据首次导入系统时，你可能需要运行此脚本数天才能完成全部地理编码。
- `UpdateGreeting.php`：每个联系人记录都有可用于邮件、邮件发送和其他形式的称呼的问候语值，例如合并信件的导出。当记录通过管理界面创建时，会构造默认问候语值（例如 尊敬的史密斯先生）。如果你更改了默认问候语，或最近导入了没有现有问候语值的联系人，请运行此脚本以更新数据库中所有联系人的值。一般来说，除非你定期导入新的联系人记录，否则没有必要通过 cron 作业运行此脚本。必须将几个变量附加到脚本以控制受影响的记录和字段（例如，在 Drupal 安装上运行的脚本可能如下所示 `http://<drupal_root>/sites/all/modules/civicrm/bin/UpdateGreeting.php?ct=Individual&gt=addressee&name=<username>&pass=<password>&key=<sitekey>`）：
    - `&ct=Individual/Organization/Household`（选择其一，必填）：确定将更新哪个联系人类型。
    - `&gt=email_greeting/postal_greeting/addressee`（选择其一，必填）：确定将更新哪个问候语值。
    - `&id=N`：选择问候语或称呼格式的选项值。这是可选的，仅在你希望将字段填充为与配置的默认值不同的问候语格式时使用。
    - `&force=1`：即使已存在值，也使用默认问候语格式更新所有记录。
    - `&force=2`：仅更新那些已设置问候语 ID 但尚未构造值的记录。这对于可能已分配默认 ID 但需要构造问候语文本的联系人记录导入非常有用。
- `EmailProcessor.php`：使用此脚本配置一个或多个电子邮件邮箱（POP3/IMAP）以定期轮询进行退信处理和电子邮件活动处理。配置为默认的电子邮件账户将用于如本章前面所述的退信处理，并将包括处理基于电子邮件的用户操作，如退订/重新订阅/选择退出。退回的电子邮件将遵循系统配置的规则，将电子邮件账户置于保留状态，让你有机会通过系统审查和更正电子邮件。电子邮件活动处理将轮询收件箱并将电子邮件作为活动记录附加到现有联系人记录（如果系统中找到该电子邮件）或创建新联系人记录（如果系统未找到该电子邮件）。这是一种强大的自动化通信跟踪方式。例如，你可以创建一个特殊的电子邮件账户，让员工在向支持者发送电子邮件时选择性地抄送，从而将与联系人的通信记录附加起来。



要识别应轮询以将电子邮件作为活动附加的账户，请在 cron 任务后追加 `&emailactivity=1`。此 cron 任务的频率取决于您发送群发电子邮件的频率，以及您希望电子邮件显示为活动的速度。在大多数情况下，按每小时运行一次就足够了。预计该脚本在未来版本中会进行一些修订，以便为两种不同的用途管理多个电子邮件账户。请查看在线文档以了解脚本使用的最新更新。

- `CiviReportMail.php`：CiviCRM 报告可以配置为定期通过电子邮件发送。例如，您可能希望每周一早上将包含上周付款汇总的捐款报告通过电子邮件发送给您的员工会计，或您的会员委员会主席可能希望每月收到一次会员概览报告。使用此 cron 作业启动邮件发送并配置其发送频率。此 cron 作业与其他作业的工作方式不同——您将为每个报告实例配置一个 cron 作业。参数通过 cron URL 传递（除用户、密码和密钥值外）：`.../bin/CiviReportMail.php?sendmail=true&instanceId=3&reset=1`。要确定报告的 `instanceId`，请访问该报告并记下 URL 中 `/instance/` 后的值。

请注意，其中一些脚本需要在运行前在代码中配置某些值。需要配置的脚本以 `.txt` 文件保存。您会看到必须配置的部分用注释代码清晰地分隔开。修改文件后，删除 `.txt` 扩展名，使其被识别为 `.php` 文件，然后配置您的 cron。例如，`UpdateMembershipRecord.php` 文件要求您确定用于发送续费提醒的 FROM 电子邮件地址。

> 自 v3.3 起，此脚本使用系统中定义的默认 FROM 电子邮件地址，因此无需手动配置地址。

脚本可以通过 cron 使用 curl 或 wget 运行。请向您的托管服务提供商咨询其首选方法和配置 cron 的任何特定要求。以下示例可能有所帮助：

- **Drupal:** `curl http://<drupal_root>/sites/all/modules/civicrm/bin/UpdateMembershipRecord.php?name=<username>&pass=<password>&key=<sitekey>`
- **Joomla!:** `curl http://<joomla_root>/administrator/components/com_civicrm/civicrm/bin/UpdateMembershipRecord.php?name=<username>&pass=<password>&key=<sitekey>`

## 安装、配置和维护

- Drupal: `wget http://<drupal_root>/sites/all/modules/civicrm/bin/UpdateMembershipRecord.php?name=<username>&pass=<password>&key=<sitekey>`
- Joomla!: `wget http://<joomla_root>/administrator/components/com_civicrm/civicrm/bin/UpdateMembershipRecord.php?name=<username>&pass=<password>&key=<sitekey>`

> wget 方法默认会在每次运行 cron 时在您的服务器上创建日志文件。对于 `civimail.cronjob.php` 等可能频繁运行的脚本，这将很快导致大量文件（尽管大小很小）保存在您的服务器上。确认 cron 作业运行正常后，使用 `-O - 'URL' > /dev/null` 开关（例如，`wget -O - 'http://...' > /dev/null`）来禁用日志记录。

请注意，所有脚本都可以通过浏览器运行。如果您在配置 cron 作业时遇到问题，请先通过在浏览器地址栏中输入包含用户名、密码和密钥值的 URL 来确认其是否正常工作。如果任何参数不正确，CiviCRM 将显示错误。一旦您确信拥有正确的 URL，请继续进行 cron 作业配置。

> 我们刚刚描述的 cron 作业设置是通过 Web 服务器使用 wget/curl 处理的。这通常是配置 cron 的更简单方法，因为它不需要命令行/SSH 访问，并且通常得到更广泛的支持。然而，通过 Web 服务器运行 cron 会增加站点负载的开销，并且如果您有大型数据库，更容易受到 PHP 超时的影响。几个 cron 作业脚本除了我们刚刚介绍的 Web 服务器选项外，还支持通过 shell 处理。如果您熟悉在 shell 中工作、拥有大型数据库，或因运行 cron 作业而导致性能下降，您应该配置 crons 通过 shell 接口运行。有关如何处理此问题的完整讨论，请参阅以下文档：`http://en.flossmanuals.net/CiviCRM/SystemConfiguration`。

此配置过程的一部分确实比其他大多数区域更技术化。然而，如果您花时间理解概念并在需要时寻求帮助来处理技术细节，您会发现 resulting 功能非常有回报。

与任何事情一样，请务必彻底测试。Cron 作业可以生成发送到电子邮件地址的日志，这是一种确保其正常运行的好方法。您还应安排定期系统审查，以确认您的 crons 按预期运行。

## 升级和维护

与任何积极开发的软件项目一样，您应预期需要定期升级到最新修订版和版本，并对系统进行定期维护。提前预测这些需求并相应规划将极大地改善您与 CiviCRM 的长期体验。

### 版本和修订升级

CiviCRM 项目遵循软件发布的常规术语：

- **Versions:** 代表具有新功能的重要软件发布
- **Revisions:** 代表次要发布并解决错误、安全和功能修复

核心开发团队每年积极发布两到三个主要版本，并在主要版本之间发布三到四个次要修订。版本号也遵循常规格式：3.2.4 = 版本 3.2，修订版 4。您可以通过 CiviCRM 网站跟踪特定版本报告的错误和问题。

由于修订通常会纠正错误或解决安全问题，您应理想地在发布后尽快实施。修订通常对数据库结构或数据需要最少的修改；因此遇到问题的潜在风险较小。

在进行任何修订升级之前，请备份您的数据库和文件系统并测试以确保备份文件的完整性。拥有可靠的数据库备份尤其重要，因为如果数据库升级脚本出现任何问题，您需要将数据库恢复到上一个版本。还建议您在实时服务器上实施之前先在开发服务器上执行升级。我们认为对于具有自定义模板和代码的站点，使用开发或暂存服务器是必不可少的。

最后，如果您对 CiviCRM 文件（包括模板 .tpl 和 CSS 文件）进行了任何修改，请确保对您的更改有良好的记录。您需要将更改与新代码库进行比较，可能需要进行调整。这对于主要版本升级尤其正确，因为文件更可能发生重大更改。

CiviCRM 为维护自定义开发工作提供了一些有用的工具：

- **Hooks**: Hooks 是 CiviCRM 代码中的占位符，您可以在其中注入自己的自定义代码。Hooks 的好处是您实际上并未修改核心文件——您的自定义代码单独维护并在适当位置注入到代码中。有关使用 hooks 的更多详细信息，请访问：http://wiki.civicrm.org/confluence/display/CRMDOC/CiviCRM+hook+specification。
- **Override Directories**: Override 目录是位于核心文件外部的文件夹，您可以在其中拥有核心文件的修改版本。CiviCRM 首先检查文件是否存在于 override 目录中，如果存在，则使用它。Override 目录可用于 PHP 和 .tpl（模板）文件。Override 目录在 **Administer | Configure | Global Settings | Directories** 中配置。
- **extras.css**: `../civicrm/css/` 文件夹包含 CiviCRM 使用的所有 CSS 文件。extras.css（默认为空）文件将覆盖其他 CSS 文件，使您能够将样式修改与核心文件分开维护。

### Joomla! 升级

Joomla! 的版本和修订升级遵循与初始安装相同的步骤，并增加一个步骤——将数据库升级到当前 CiviCRM 版本。您必须将新版本上传到服务器上的某个位置（我们建议使用 /tmp/ 目录），解压它，并使用 **Extensions | Install/Uninstall** 工具中的 **Install from Directory** 选项（请参阅上面的 Joomla! 安装步骤以获取更详细的演练）。

触发安装后，您应收到数据库必须更新到当前版本的通知。点击升级后，您应看到升级成功消息、访问 CiviCRM 仪表板页面的链接（菜单将在此处重建），以及可能有关可能影响您站点的任何配置更改的消息。

如果升级未成功完成，您将需要进行故障排除以确定原因。

一个可能的问题可能是文件系统上的权限限制禁止安装脚本覆盖现有文件。这通常很容易注意到，因为数据库升级脚本不会报告从上一个版本到当前版本的更改，并且页脚区域和 Joomla! 扩展管理器中的版本信息不会反映新版本。如果发生这种情况，您将需要解决文件系统上的权限问题，或先卸载 CiviCRM 然后重新安装。请注意，卸载/重新安装会保留您的数据，但您需要查看配置设置并必须重置指向 CiviCRM 页面的任何菜单项。




在数据库升级步骤中，您也可能会遇到问题。对于大型数据库或低端服务器环境，脚本可能会在处理过程中超时，导致数据库只升级了一半。如果发生这种情况，您需要从备份中恢复数据库并重新启动升级。如果问题仍然存在，您可能需要通过 PHPMyAdmin 或命令行界面手动执行 SQL 脚本。您可以在安装包的 `/civicrm/sql/` 文件夹中找到这些脚本。

如果数据库升级脚本产生带有回溯日志的错误，则很可能是当前数据库中的结构或外键关系存在问题。回溯错误将为问题的来源提供一些指导。您需要在重试安装过程之前解决当前数据库中的问题。

如果您在数据库升级方面仍然遇到问题，请完成以下步骤，以确保当前（升级前）版本的数据库模式结构正确：

- 1. 仅备份当前版本数据库的数据。这可能要求您重新导入在此过程开始时所做的备份，然后仅导出数据（不含结构）。
- 2. 创建一个新的空数据库。
- 3. 从安装包中导入当前 CiviCRM 版本的数据库结构。该结构位于 `.../civicrm/sql/civicrm.mysql`。将其导入到干净的数据库可确保您拥有当前版本的正确结构。
- 4. 将当前版本的数据导入到该干净数据库中。现在，您应该拥有当前站点数据，且结构干净。
- 5. 现在，使用这个包含干净结构和当前站点数据的新数据库重新运行升级过程。这可能需要在当前生产数据库中转储表，并用这个新数据库中的表替换它们。
- 6. 继续执行安装和数据库升级。

> 请注意，如果您的站点包含自定义数据，这些数据将存储在自己的表中，不会包含在上一步导入的干净结构中。您需要导入自定义表的结构和数据。

这些步骤应能确保 CiviCRM 数据库升级脚本在当前数据库中使用准确的结构运行。

完成升级后，打开您的 `civicrm.settings.php` 文件，并确认站点密钥值完好无损（用于 cron 作业，如前所述）。

## Drupal 升级

Drupal 的升级过程需要多个步骤才能成功完成。与“安装 CiviCRM”部分中的“在 Drupal 中安装”小节一样，我们将介绍两种升级方式：使用浏览器和 FTP，以及使用 Drush：

- 1. 按照以下步骤将站点设为离线：
  - 以用户 1 身份登录。
  - 转到 **管理 | 站点配置 | 站点维护**。
  - 点击 **离线**。根据需要查看并编辑 **站点离线消息**。
  - 点击 **保存配置**。

- 2. 如果您的首页是 CiviCRM 页面：
  - 点击 **管理 | 站点配置 | 站点信息**。
  - 记下 **默认首页**，以便在升级后可以恢复，或许可以通过截图方式记录。
  - 将 **默认首页** 更改为 **node**。
  - 点击 **保存配置**。

- 3. 如果您正在使用任何 CiviCRM 集成或扩展模块：
  - 点击 **管理 | 站点构建 | 模块**。
  - 记下正在使用的集成或扩展模块，也许可以通过截图方式。
  - 取消选中所有相关模块的 **已启用？** 复选框（例如，**CiviMember 角色同步**）。
  - 点击 **保存配置**。

- 4. 列出并记下当前启用的命令行脚本：

   ```bash
   $ ls /path_to_docroot/sites/all/modules/civicrm/bin/*.php > /path_to_safe_dir/SavedListOfEnableScripts.txt
   ```

- 5. 下载新的 CiviCRM 文件：
  - 您可以使用浏览器从 http://sourceforge.net/project/showfiles.php?group_id=177914 下载文件，然后使用 SFTP 或 FTP 上传到服务器上的 /path_to_docroot/sites/all/modules 目录。
  - 或者，使用浏览器从 http://sourceforge.net/project/showfiles.php?group_id=177914 下载文件以查找当前版本的名称，然后在命令行执行以下操作：

    ```bash
    $ wget -O /path_to_docroot/sites/all/modules/civicrm.tar.gz http://sourceforge.net/projects/civicrm/files/civicrm-stable/3.2.0/civicrm-3.2.0-drupal.tar.gz/download
    ```

- 6. 使用 Drush：

   ```bash
   $ cd /path_to_docroot/

   $ drush civicrm-upgrade --tarfile=sites/all/modules/civicrm-3.2.0-drupal.tar.gz
   ```

- 7. 使用非 Drush 方法：
  - 备份您的 CiviCRM 和 Drupal 数据库。
  - 例如，在命令行中执行，

    ```bash
    $ mysqldump -uCivicrmDbUser -pCivicrmDbPassword -h localhost --databases yourDomain_dru yourDomain_civ > yourDomain.backup.mysql
    ```

  - 备份您的 CiviCRM 文件：
  - 例如，在命令行中执行，

    ```bash
    $ cd /path_to_docroot/sites/; tar -czf civicrm.files.old.tgz all/modules/civicrm yourDomain/files/civicrm
    ```

  - 删除 CiviCRM 核心文件：

    ```bash
    $ cd all/modules
    $ rm -rf civicrm
    ```

- 8. 解压文件，将 `3.2.0` 替换为上面相应的文件名：

   ```bash
   $ tar -xzf civicrm-3.2.0-drupal.tar.gz
   ```

- 9. 验证默认或 yourDomain.org 站点目录中的文件目录权限是否仍然正确（视情况而定）：

   ```bash
   $ chmod -R 777 /path_to_docroot/sites/yourDomain.org/files
   ```

- 10. 通过浏览器运行升级，如下所示：
  - 以管理员身份登录您的站点：
  - 如果启用了简洁 URL，请访问 `http://yourDomain.org/civicrm/upgrade?reset=1`；如果未启用简洁 URL，请访问 `http://yourDomain.org/?q=civicrm/upgrade&reset=1`。
  - 点击 **立即升级**。
  - 如果出现任何问题，请前往相应的 CiviCRM 论坛寻求帮助：`http://forum.civicrm.org/index.php/board,8.0.html`。
  - 如果没有问题，点击 **返回 CiviCRM 首页** 以重建 CiviCRM 菜单并进入仪表盘。

- 11. 重新启用 CiviCRM 集成和扩展模块：
  - 点击 **管理 | 站点构建 | 模块**。
  - 在您之前禁用时记下的相应集成和扩展模块旁边勾选 **已启用**。
  - 点击 **保存配置**。
  - 如果您已重新启用 Views 集成：
    - 点击 **管理 | 站点构建 | Views | 工具**。
    - 点击 **清除 Views 缓存** 按钮。

- 12. 如果合适，将 CiviCRM 恢复为首页：
  - 点击 **管理 | 站点配置 | 站点信息**。
  - 在 **默认首页** 中，将值更改为升级过程中早先保存的值。

- 13. 将站点恢复为生产状态：
  - 点击 **管理 | 站点配置 | 站点维护**
  - 勾选 **在线**
  - 点击 **保存配置**

- 14. 检查 CiviCRM 功能，确保其正常运行。

- 15. 重新启用您在升级过程中之前使用并保存的脚本：

   ```bash
   $ cd /path_to_docroot/sites/all/modules/civicrm/bin
   $ cat /path_to_safe_dir/SavedListOfEnableScripts.txt
   ```

- 16. 对于列出的每个脚本文件，运行类似以下命令行中的命令，删除 .txt 扩展名以重新启用脚本：

   ```bash
   $ mv scriptName.php.txt scriptName.php
   ```

- 17. 如果您遇到困难，请查看 http://wiki.civicrm.org/confluence/display/CRMDOC/Upgrade+Drupal+Sites+to+3.2#UpgradeDrupalSitesTo3.2-UpgradeTroubleshooting 上的故障排除建议。如果您仍然遇到问题，请访问 CiviCRM 升级论坛：http://forum.civicrm.org/index.php/board,8.0.html。

完成升级后，您应该考虑是否需要启用任何新组件或配置任何新的 CiviCRM 权限。




## 将安装迁移到新服务器

如果您计划将网站迁移到新的托管环境或从开发服务器迁移到生产服务器，您需要遵循几个步骤，以确保 CiviCRM 在新环境中正确配置。以下内容代表了迁移网站时需要考虑和处理的重点问题；有关迁移网站的完整步骤讨论可在网上找到：http://wiki.civicrm.org/confluence/display/CRMDOC/Moving+an+Existing+Installation+to+a+New+Server+or+Location。

将网站的文件和数据库传输到新位置后，您必须更新多个文件以反映更改。CiviCRM 在多个位置引用文件根目录和网站的完整 URL。请按照以下步骤操作：

- 1. 使用新的目录和 URL 位置修改 civicrm.settings.php。在 Drupal 安装中，该文件通常位于 /sites/default/civicrm.settings.php。Joomla! 维护此文件的两个版本，两者都必须更新（并且引用略有不同）。您可以在以下位置找到它们：/administrator/components/com_civicrm/civicrm.settings.php 和 /components/com_civicrm/civicrm_settings.php。
- 2. 删除缓存的模板文件，其位置为：
  - Drupal：<drupal-root>/sites/default/files/civicrm/templates_c/*
  - Joomla!：<joomla-root>/media/civicrm/templates_c/*
- 3. 以管理员身份登录您的网站并重置系统配置中的基本 URL 和资源目录。对于 Drupal 安装，请浏览至 http://<drupal_site>/index.php?q=civicrm/admin/setting/updateConfigBackend&reset=1。对于 Joomla! 安装，请浏览至 http://<joomla_site>/administrator/index2.php?option=com_civicrm&task=civicrm/admin/setting/updateConfigBackend&reset=1。
- 4. Joomla! 安装还必须手动修改 /administrator/components/com_civicrm/civicrm/civicrm.config.php，以正确引用管理员文件夹中 civicrm.settings.php 的位置。
- 5. 重建 CiviCRM 菜单，其中可能包含完整 URL，现在必须反映新位置：
  - Drupal：http://<drupal_site>/index.php?q=civicrm/menu/rebuild&reset=1
  - Joomla!：http://<joomla_site>/administrator/index2.php?option=com_civicrm&task=civicrm/menu/rebuild&reset=1
- 6. 如果网站中的任何资源引用完整 URL 或文件位置，您可能需要更新这些资源（例如，如果您的 Drupal 导航菜单包含指向个人资料的链接）。
- 7. 如果您使用覆盖目录（`PHP` 或 `.tpl`），您将需要重置它们以反映新的文件位置。转到 **Administer CiviCRM | Configure | Global Settings | Directories**。同时访问 **Resource URLs** 配置页面以确认值设置正确。

完成迁移后，请务必测试网站！点击浏览 CiviCRM 并确认页面按预期加载，并且 URL 保持在您的新网站上（如果存在过时的链接，可能会将您发送到旧域名或子目录）。

## 系统维护

与任何数据库系统一样，需要定期执行一些维护步骤。本节的重点是 CiviCRM 特定的基本维护步骤。本章不涉及服务器和 MySQL 数据库维护的更广泛主题。根据数据库的大小以及是否使用托管托管服务，您应该了解数据库和托管环境的基本维护步骤。此外，我们不会涉及与可扩展性相关的问题和维护。但是，随着系统的增长，您应该意识到对 MySQL 数据库性能和大小的影响。

对系统最重要的维护步骤是将其保持更新到最新版本，并且不要在主要版本升级方面落后太多。由于本章前面已讨论了执行此操作的步骤，因此我们在此不详细介绍；我们只是因为其重要性而重复：

- - 修订版发布可能包含与安全相关的补丁，这对于维护安全可靠的数据库至关重要。
- - 修订版发布可能包含错误修复补丁，这对于维护稳定且完全运行的系统至关重要。
- - 主要版本发布包含新功能和功能。如果新功能不是您需要或使用的，您可以决定在首次发布时不升级。这是可以的，也很常见。但核心开发团队只会积极开发和修补当前稳定版本，这意味着旧版本中发现的安全或错误问题可能得不到解决。一般来说，您应避免在主要版本升级周期中落后太多。此外，新版本中有大量优秀功能，您会希望从中受益！

另一个重要的定期维护步骤是处理重复的联系人记录，这些记录会以一种险恶的方式悄悄进入您的系统。如果您有多个表单从公共网站收集信息，例如活动注册和捐款表单，则尤其如此。

重复记录通常是由于现有联系人使用替代电子邮件地址或昵称完成在线表单而导致的。由于替代数据，记录与现有记录不匹配，从而导致重复。

幸运的是，CiviCRM 提供了一些出色的去重和合并数据库的工具，位于 **Contacts | Find and Merge Duplicate Contacts** 下。这些工具的完整范围将在本书后面详细介绍。就维护考虑而言，您应安排定期对整个数据库进行去重。系统的大小和用途将决定维护应多频繁进行。实施系统后，从上线日期起一个月安排一次初始去重/合并。根据去重规则发现的潜在重复数量，您可以相应地安排下一次维护审查。

> 去重/合并工具的新功能目前正在开发中，其中包括性能改进、白名单管理以及自动化处理（在整个数据库中运行去重规则并自动合并）。请关注 CiviCRM 博客和发布说明以获取更多信息。

在介绍初始系统配置时，我们介绍了在服务器上创建 cron 作业以触发自动事件和操作。在大多数情况下，这些作业将继续运行而无需任何干预或维护。然而，定期检查以确保它们确实正常运行并且脚本已完成是个好主意。在创建 cron 作业时，您可以选择将日志发送到电子邮件地址。日志的详细信息不多，但会提供脚本已成功完成的验证。您应养成定期检查配置的电子邮件的习惯，以确认脚本正常运行。

这些基本维护步骤应该足以让您的系统平稳运行。

## 制定备份策略和程序

没有关于维护的章节是不完整的，如果不提及备份策略和程序的话。

- - **总体而言，数据库备份比文件系统备份更重要：** 为什么？所有您的数据，很可能每天都在更改和处理，都存储在数据库中。文件系统，主要是应用程序代码，很少更改。请理解这是一个一般性陈述。如果您的网站和 CiviCRM 定期使用文件上传工具，您也希望确保这些工具也能定期备份。这并不是说您不应该制定文件系统备份策略；我们只是建议您可能不需要像数据库那样频繁地备份文件系统。
- - **备份到多个位置：** 正如您不想仅依赖托管提供商作为唯一的备份解决方案一样，您也不想将办公室局域网或托管服务器作为备份的唯一存储库。在可能的情况下，建立一个系统以至少在两个位置维护定期备份。

使用这些基本规则并投入您的时间和资源，以确保在意外丢失的情况下您的数据得到保存。

> Joomla! 和 Drupal 都有多个扩展程序可生成定期数据库转储，将其保存到服务器，并将压缩文件作为附件发送到配置的电子邮件地址。对于较小的 CiviCRM 实施，这提供了一种自动生成备份并通过电子邮件“异地”发送的简便方法。考虑创建一个 Gmail 帐户（提供充足的存储空间）来捕获和存储备份。虽然您应定期将备份下载到本地服务器或其他异地空间，但这可以提供一种经济且易于实施的备份计划。此解决方案的一个警告是，您可能无法通过电子邮件发送数据库的加密版本，这意味着您有一定的暴露风险。这应被考虑并纳入您的整体策略和程序中。一些可用的扩展程序是：
> 
> - Drupal（db）：Backup and migrate 模块：`http://drupal.org/project/backup_migrate`
> - Drupal（files）：Backup and migrate files 模块：`http://drupal.org/project/backup_migrate_files`
> - Joomla!（db 和 files）：Akeeba backup：`http://extensions.joomla.org/extensions/access-a-security/site-security/backup/1606`

![](img/76a9a15ee6002f74a917dd7fbdfc8b1c_145_0.png)





## 第4章：CiviCRM 基础：浏览系统和处理联系人

# CiviCRM 基础：浏览系统和处理联系人

到目前为止，你已经完成了分析组织结构和流程与 constituent 管理相关的关键步骤。你也已在首选的 CMS 环境中安装并配置了 CiviCRM。此外，你已经了解了本书中将贯穿使用的案例研究。现在是时候熟悉 CiviCRM 界面并开始处理联系人记录了。

无论你是否正在实施 CiviCRM，只是想了解更多关于该软件的信息，还是希望更深入地了解其提供的工具，你最终来到这里是因为你代表某种拥有 constituent 的组织。那些 constituent 可能是会员、志愿者、捐赠者、活动参与者，或是你接触到的个人和组织。然而，在你的核心目的和 CiviCRM 的核心中，都是你互动并记录信息的联系人。

联系人记录构成了所有相关记录生成的中心枢纽。你将以各种方式收集、检索和报告这些相关记录，但最终，它们都连接到一个联系人。

![](img/76a9a15ee6002f74a917dd7fbdfc8b1c_148_0.png)

在本章中，我们将执行以下操作：

- 熟悉 CiviCRM 界面和导航工具
- 发现联系人记录，了解如何处理联系人数据，并开始组织和分类我们的记录
- 了解可用的强大搜索功能
- 从搜索结果中对多个联系人采取行动
- 审查替代工作流程选项并对记录进行维护

## 界面介绍

在深入联系人记录之前，让我们简要回顾一下 CiviCRM 界面。导航到你在 Drupal 或 Joomla! 中的 CiviCRM 安装，或使用 demo.civicrm.org 提供的在线演示，开始探索。

CiviCRM 界面由三个主要区域组成，即水平导航菜单、左侧边栏和主要内容区域。根据你使用的是 Drupal 还是 Joomla!，这三个区域看起来会略有不同，但它们本质上是相同的，并提供相同的功能。

本章中的屏幕截图取自 Joomla! 安装。Drupal 用户可能会注意到以下差异：

- 水平导航菜单将位于屏幕顶部，取代标准的 Drupal 管理菜单栏（除非你的安装已将其从此默认屏幕位置移开）。
- 左侧边栏由可配置的块组成。根据你的设置，你可能看不到 Joomla! 屏幕截图中存在的所有块。
- Joomla! 具有一致的管理界面，而 Drupal 界面默认继承网站主题。因此，你安装中的颜色、字体、宽度和其他设计和布局元素将反映你的 Drupal 主题。

如果你希望为管理界面使用不同的主题，你可以使用 Administration theme 模块（http://drupal.org/project/admin_theme）为 Drupal 管理区域选择一个主题，并使用 CiviCRM Theme 模块（http://drupal.org/project/civicrm_theme）为 CiviCRM 管理区域选择一个主题。

![](img/76a9a15ee6002f74a917dd7fbdfc8b1c_149_0.png)

水平导航栏（屏幕截图中标记为 A）是可配置的（来自 Administer | Customize | Navigation Menu），并将反映你已启用的 CiviCRM 组件。栏的最左侧部分包含一个快速搜索框，当你开始输入时将填充记录选择。根据你的配置设置，它可能搜索姓名、昵称或电子邮件。

![](img/76a9a15ee6002f74a917dd7fbdfc8b1c_150_0.png)

左侧边栏（标记为 **B**）将包括一个用于创建新记录的下拉列表、你最近访问的项目的列表、你的联系人仪表板链接，以及一个用于创建新个人记录的简短表单。对于使用 Drupal 的用户，这些可配置的块可以通过 Drupal **Administer** 菜单访问。主联系人区域（标记为 **C**）是你浏览系统时所有操作发生的地方。

如果你是 CiviCRM 的新手，花时间探索导航菜单并熟悉可用的选项。你会很快发现该软件功能丰富。越快熟悉术语和工具，你就越能利用其提供的强大功能。

## 联系人

简单来说，联系人是你互动的任何实体。它可能是个人、企业、政府机构、家庭、组织分会、地区或你在系统中收集数据的任何其他人。

### 个人、组织和家庭

由于所有联系人实体并不相同，你与它们互动的方式也会有所不同，因此 CiviCRM 将联系人分为三种主要类型，并可进一步细分为子类型：

- **个人联系人：** 这些联系人代表个人。在你处理联系人记录时，你将记录诸如名字、姓氏、出生日期、性别以及仅与个人互动相关的其他类型数据。此外，个人是唯一可能在你的 Joomla! 或 Drupal 网站内拥有对应用户账户的联系人类型；毕竟，“企业”不会登录网站，为企业工作的个人才会登录。CiviCRM 提供了工具，允许具有足够权限的个人对他们关联的组织记录采取行动，我们将在后面讨论。然而，了解个人联系人类型的这一独特方面很重要。
- **组织联系人：** 这些可能代表非营利组织、企业、社区组织、你组织的地区分会或任何其他“有组织的”实体。在处理组织时，你记录的是组织名称、法定名称以及此类型特有的其他数据。
- **家庭联系人：** 这些联系人也许是最不明显的，因为它们旨在提供将个人记录组合在单一伞下并跟踪可能适用于整个组的信息的能力。这种类型最明显的好处是能够让多个个人共享一个地址。例如，如果联系人搬家，共享地址只需更新一次。如果已婚夫妇及其三个孩子在你的系统中被跟踪，你将希望创建一个家庭记录并录入他们的家庭地址。此外，每个家庭成员的个人联系人记录将表明他们共享家庭地址。除了减少填写地址字段的次数并限制拼写错误的可能性外，你在打印邮件标签或导出列表时还能够将个人组合在单一家庭记录下。这可以节省大量成本，因为你将邮件件数从五个减少到一个，如前例所示。

### 联系人子类型

在大多数情况下，我们刚才描述的三种主要联系人类型足以满足组织需求。当你需要收集和存储这些类型内子组的不同信息或为子组定义特定关系时，你可以考虑将记录类型进一步细分为联系人子类型。每个子类型从三种主要类型之一继承其基本“实体身份”，但在其他方面可以被视为独特。

> 大都会大区食品储藏室协会（FPAGM）为大都会市及其周边地区的食品储藏室提供服务和支持。每天，他们与组织成员的 30 到 40 个食品储藏室互动。此外，他们还与捐赠剩余食品供食品储藏室分发的餐馆、杂货店和便利店互动。
> 很明显，食品储藏室和捐赠食品的企业是截然不同的实体。他们收集的数据性质、通过邮件和电子邮件的互动方式，以及将运行的报告类型将取决于组织是储藏室还是食品供应商。因此，FPAGM 决定在主要组织类型下创建两个联系人子类型。





联系人子类型是通过以下路径创建和管理的：**Administer | Option Lists | Contact Types**。从此屏幕中，如以下屏幕截图所示，您可以重命名现有类型（包括三个主要类型），创建新类型，并为每种类型分配图标，以便在查看记录时在视觉上区分它们。

![](img/76a9a15ee6002f74a917dd7fbdfc8b1c_152_0.png)

## 规划您的联系人类型

在配置和构建您的 CiviCRM 实现时，仔细考虑您如何处理不同类型的联系人非常重要。联系人类型可以被视为系统中最高级别的分类，您所做的选择将在整个系统的其余部分产生影响。

在规划联系人类型时，请考虑以下几点：

- 不要过度使用联系人子类型。您将有大量其他方式对记录进行分类和细分。子类型最适合在联系人之间存在明显差异且触及实体核心性质和角色时使用。
- 联系人只能分配一个联系人类型；不要创建可能导致子类型之间混淆或重叠的结构。在食品储藏室的示例中，您不希望为食品供应商、杂货店和便利店创建子类型，因为杂货店和便利店都是食品供应商，而这三种子类型会造成不确定性，无法确定某个企业最适合放在哪里。最好为食品供应商创建一个单一的子类型，并使用其他方式（例如标签或自定义字段）在此一种实体类型内进一步分类。
- 如果您计划从现有数据库迁移数据，请务必花时间将现有记录整理到您计划在 CiviCRM 中使用的类型和子类型中。CiviCRM 提供的联系人导入工具要求您一次导入一种联系人类型。您希望在开始导入之前确定联系人结构并相应地准备导入数据。
- 联系人子类型的主要好处之一是能够为一种或多种类型创建一组自定义数据字段。自定义数据字段是您创建的用于扩展 CiviCRM 原始提供的核心字段的字段。这是一种有用的规划思路：我是否计划为这种类型的组织收集数据，而我不需要或这些数据明显不适用于另一种类型？如果是这样，可能值得创建一个子类型。

您在创建联系人记录时分配联系人子类型。如果现有联系人未选择子类型，您可以在编辑时指定一个子类型。但是，如果您已选择子类型并存储了与该类型关联的任何自定义数据，您将无法再更改子类型（除非该子类型的全部自定义字段集已被永久删除）。例如，FPAGM 创建了一个自定义数据字段来跟踪食品储藏室服务的客户日均数量。该字段附加到食品储藏室子类型，因为它仅与这些联系人相关。如果我们创建一个新的组织记录，将其分配为食品储藏室子类型并保存组织联系人后，该记录现在就锁定在该子类型上；我们不能在以后决定将记录更改为食品供应商子类型。这种系统行为可确保防止意外数据丢失或孤立数据。如果在以后您决定完全删除与子类型关联的自定义数据字段，您将再次能够将联系人重新分配给不同的子类型。基本上，CiviCRM 会锁定子类型以防止数据丢失——如果记录中没有存储子类型特定的数据，您可以无风险地更改该值。

在讨论联系人类型时，还有另一个重要的考虑因素需要思考，特别是如果您有一个复杂的组织，有多个管理用户将使用 CiviCRM。

正如我们很快就会看到的，使用 CiviCRM 的很大一部分工作涉及将相关记录与联系人记录连接起来。例如，您可能会跟踪与个人的电话和会议、企业的捐赠、家庭的会员资格，或为这些联系人中的任何一个开立案件。非常经常地，会在相关联系人之间建立连接，并且在每种情况下，您都必须决定捐赠、会员资格或其他记录应附加到何处。

John Smith 是 ACME 便利店的业主，他致电 FPAGM 安排捐赠 50 磅面包供食品储藏室分发。FPAGM 员工将在 CiviCRM 中将其跟踪为实物捐赠。提出的问题是，捐赠应记录在 John Smith 名下（毕竟他是业主和主要联系人），还是记录在 ACME 便利店名下？

在大多数情况下，相关记录应附加的位置非常明确。然而，在存在混淆和模糊可能性的地方，您希望建立明确的政策和程序来帮助员工确定数据的正确位置。

考虑以下问题：

- 数据的官方“来源”在哪里？如果将来收到类似数据，它将在何处被跟踪？在前面的示例中，我们会将捐赠记录在组织（ACME 便利店）名下，而不是 John Smith 名下。真正做出贡献的是企业，而不是个人。
- 谁实际参与了该活动？例如，谁将参加活动，或正式的税务收据应开具给谁？一个常见的混淆领域是组织主办贸易展作为会议的一部分。您的参展商是企业本身还是代表企业的个人？这可能很棘手，因为“企业”不会参加活动，参加的是人，而且可以为将展览相关细节记录在组织记录中提出很好的论据。

无论您如何解决这些潜在的混淆领域，都应努力保持一致性。您输入数据的方式将直接影响您能够从系统中提取的内容。

一旦您定义了联系人子类型（如果有）并思考了将为员工制定的政策，请花时间手动创建几条包含真实数据的记录来测试您的结构和政策。这可能有助于您识别并纠正配置的任何意外后果。

## 核心信息字段

让我们进入一个联系人记录，开始了解这种联系人及其相关记录的理念在 CiviCRM 中是如何体现的。如果这是您第一次使用 CiviCRM，请将光标放在 CiviCRM 菜单栏左侧的搜索框中，如以下屏幕截图所示，然后单击 Enter 以查看所有记录：

至少，您应该看到一条对应于您网站用户账户的记录（您可能也在左侧边栏最近项目列表中看到过该记录）。进入该记录，或任何其他记录，以查看联系人详细信息。

如您所见，联系人记录通过一系列选项卡显示，对应于您可能收集的不同类型的数据。

> 联系人记录中显示的选项卡取决于两个地方的配置设置：**Enable Components**（Administer | Configure | Global Settings | Enable CiviCRM Components）和站点首选项（Administer | Configure | Global Settings | Site Preferences）。如果您没有看到预期的选项卡，请确保该组件已启用，并且您已指示它应在联系人记录页面上可见。

![](img/76a9a15ee6002f74a917dd7fbdfc8b1c_155_0.png)

我们此时不会讨论所有可用的选项卡，而只涵盖那些直接与联系人记录本身相关的选项卡，包括通信跟踪、与其他记录的关系/连接以及核心分类工具。





## CiviCRM 基础：系统导航与联系人管理

默认的摘要标签页可能是最重要的标签页，因为它跟踪联系人姓名、通信详情和偏好、地址以及人口统计信息（如果是个人联系人类型）。点击标签页顶部的编辑按钮即可查看联系人所有可用的字段。你会注意到编辑屏幕被划分为字段分组面板，对应于你收集的不同类型的信息。

![](img/76a9a15ee6002f74a917dd7fbdfc8b1c_156_0.png)

### 联系人详情

联系人详情面板包含有关实体的基本信息，包括以下内容：

- 联系人姓名，包括昵称，这可能作为搜索的替代值很有用。例如，如果你数据库中某家非营利组织的名称通常使用缩写，你可能希望为正式用途拼写出全名，并将缩写放在昵称字段中。它可以随记录一起轻松导出，并在使用搜索栏时找到。
- 请注意，如果已配置任何联系人子类型，它们将在此区域可供选择（联系人类型下拉菜单）。一旦你选择了子类型并将数据保存到与该子类型关联的任何自定义字段中，更改类型的能力将被锁定。仅列出与主类型对应的子类型。例如，如果你正在查看个人记录，则只有个人子类型可用。
- 个人记录有一个特殊的当前雇主字段，可在个人与其受雇的组织之间创建关系。当前雇主字段在联系人摘要标签页中突出显示，并且可以轻松地与个人记录一起导出。请注意，如果你在此字段中输入一个不存在的组织名称，将会创建该组织联系人记录。你可以利用这一点来减少数据录入。
- 通信选项包括电子邮件、电话、即时通讯、网站和 OpenID。这些字段中的每一个都允许你添加多个值并对其进行分类。例如，一个人可能在家有座机和手机号码，在工作有座机、手机和传真号码。或者，某人的记录中可能记录了工作电子邮件和个人电子邮件。如果你的网站不需要跟踪 OpenID，你可以通过 管理 | 配置 | 全局设置 | 网站偏好 从界面中将其移除。
- 如果输入了多个通信值，其中一个将被标记为主要值。主要值可以更轻松地导出，无论其类型如何。例如，你的一些联系人可能希望将家庭电话标记为主要，而其他人则希望将工作电话或手机标记为主要。主要标记与记录的来源或类型无关；它只是表示其首选选项。
- 电子邮件字段有两个额外的复选框是其功能独有的 — **暂停** 和 **批量邮件**。标记为 **暂停** 的电子邮件地址表示它可能是一个有问题的地址。虽然你可以手动将地址标记为此类，但更可能的是该字段因 CiviMail 退信邮件处理器而被标记为此方式。邮件处理器是一个可选工具，可以设置以检索任何退回（被退回为无法投递）的电子邮件并将其标记为 **暂停**。这为你提供了审查记录并确定问题来源的机会。在标记为 **暂停** 期间，它们将被排除在任何未来的 CiviMail 邮件之外。批量邮件标志指定一个电子邮件地址作为通过 CiviMail 生成的任何批量电子邮件的首选邮件地址。例如，一个人可能希望将其工作电子邮件指定为主要地址，因为他们希望立即收到直接通信。然而，发送给组织所有成员的每周电子邮件通讯应发送到该人的家庭电子邮件，以免 clutter 他们的收件箱。
- 来源字段可用于记录此特定联系人如何参与你的组织，或如何被添加到系统中。如果你的组织的增长主要归功于口碑网络，这可能特别有用。例如，你可能会记录 Jane Doe 是通过你的董事会成员之一的外联而被介绍给该组织的。
- 外部 ID 记录来自某个外部来源的唯一 ID。如果你从另一个数据库（如 MS Access）导入记录，你可能希望将联系人的主键 ID 字段导入此字段。虽然一旦你完全在 CiviCRM 中工作时通常不会使用它，但它提供了一种将记录追溯到先前数据库系统中的原始来源的有用方法。这也可在初始导入过程后用于数据完整性分析。在拥有更复杂 IT 基础设施的大型组织中，它可能使与其他软件工具的持续数据同步变得更容易。

### 自定义数据

正如我们在 *第 6 章，更好的沟通* 中将看到的，CiviCRM 允许你创建自定义字段集并指示它们应内联显示还是在联系人表单的自己的标签页中显示。如果为所有联系人或你正在编辑的特定联系人类型定义了一些自定义内联字段，这些字段将直接出现在联系人详情面板下方。

### 地址

与通信选项类似，你可以创建多个地址集，对其进行分类（家庭、工作等），并将其中一个标记为主要地址。有几件重要的事情需要注意：

- 现有地址可被标记为账单地址，或你可以创建新地址并使用 **位置类型** 选项将其指定为账单地址。前者如果单个地址用于多种用途则很有用。账单地址将在联系人通过在线信用卡支付捐赠、活动或会员费时使用。
- 个人联系人可以选择家庭记录的地址作为地址集。为此，开始输入家庭名称并从下拉列表中选择它。如果家庭记录不存在，你可以通过输入所需家庭记录的名称来创建新记录。联系人只能有一个关联的家庭地址。
- 每个记录对于每种地址类型只能有一个地址集。换句话说，你不能在单个联系人记录中将三个地址指定为 **工作**。这看起来可能有些限制，因为个人拥有两个工作地点或家庭和第二家庭地址并不罕见。然而，当你导出记录时，这种限制的用处变得明显，因为你可以选择仅导出记录的一种地址类型。具有相同类型的多个地址集将需要重复记录或重复字段列，这两种方式都难以处理。如果你需要跟踪多种相似类型的地址，请考虑添加标记为 **Home1**、**Home2**、**Home3** 等的选项值（**管理 | 选项列表 | 位置类型**）。

![](img/76a9a15ee6002f74a917dd7fbdfc8b1c_159_0.png)

> 当你首次安装 CiviCRM 时，地址面板包含所有可用字段，包括你可能不希望显示的一些字段。访问 **管理 | 配置 | 全局设置 | 地址设置** 以关闭此面板中的元素。例如，你可能只想跟踪两行地址，因此你将禁用 **附加地址 2**。同样，你不太可能需要手动编辑记录的经纬度值，因为如果已为你的网站启用地址地理编码，这通常会自动处理。

我们可以在以下屏幕截图中看到 **地址** 面板：

![](img/76a9a15ee6002f74a917dd7fbdfc8b1c_160_0.png)

### 通信偏好

现在你已建立联系人记录并记录了与他们沟通的各种方式，CiviCRM 提供了进一步分类（和限制）你的通信方法的工具。这些偏好分为四个区域：

- 问候语偏好在发送电子邮件、生成标签或导出联系人时使用，可以使用记录中的字段（令牌）或完全自定义。
- **隐私设置** 显示个人是否更喜欢不通过某种方式被联系。**隐私** 设置不会锁定对通信详情的访问；它们只是允许 CiviCRM 用户快速看到联系人已请求不以所选方式被联系的工具。如果被选中，隐私设置会在 **联系人摘要** 标签页中相应方法旁边生成一个醒目的“停止”图标。
- 同样，**首选方法** 用于你的内部目的，以便促进和改善与联系人的沟通。





-   电子邮件设置提供了一个退出选项标志和格式偏好：
    -   退出选项标志（**禁止群发邮件（用户选择退出）**）符合美国的**反垃圾邮件法案（CAN-SPAM Act）**，通常仅在联系人明确表示不希望接收群发邮件时使用。通过 CiviMail 生成的群发邮件必须包含退出代币（要么将用户引导至一个可供其选择退出的网页，要么提供一个用于自动处理的电子邮件地址）。您很可能很少会通过界面选择该选项。如果您这样做了，请注意，这将自动将该用户从 CiviCRM 生成的任何邮件中移除。
    -   **电子邮件格式**字段用于确定用户偏好接收文本还是基于 **HTML** 的电子邮件。默认值为**两者**，这意味着使用 HTML 和文本版本生成的电子邮件都将发送给联系人。请注意，用户的电子邮件客户端设置将决定实际显示哪一个版本。您可能需要定期监控此字段，以确定用户是否明确要求通过文本接收电子邮件，因为这将要求您生成两种形式的电子邮件以便通过 CiviMail 发送。

![](img/76a9a15ee6002f74a917dd7fbdfc8b1c_161_0.png)

## 人口统计信息

个人联系记录将包含一个**人口统计信息**窗格，其中可以记录**性别**、**出生日期**和**逝世日期**。在进行搜索以导出邮件列表联系人时，您可以排除已故联系人，以确保他们不会收到邮件。

![](img/76a9a15ee6002f74a917dd7fbdfc8b1c_161_1.png)

## 删除联系人

在联系人摘要视图中，您会注意到选项卡式界面上方和右侧有一个**删除**按钮。CiviCRM 具有联系人回收站功能；当您单击此按钮删除联系人时，它并不会将其或任何相关记录从系统中完全移除，而是将其标记为已删除。已删除的记录可以使用高级搜索工具进行搜索；在**基本条件**窗格底部附近可以找到**在回收站中搜索（已删除的联系人）**选项。搜索到已删除的联系人后，您可以将其永久删除或恢复。

此功能是保护数据免遭意外删除的有效方式。但是，与任何安全网一样，您不应过度依赖它。切勿在删除记录时怀有稍后恢复它的意图。已删除的记录应被视为完全删除（并且在生成搜索和报告时，它们看起来就像被系统实际删除了一样）。

请注意，您可以通过**管理 | 配置 | 全局设置 | 其他设置**禁用系统中的回收站功能。

## 标签和分组

您可能已经注意到，我们跳过了联系人摘要编辑屏幕底部的**标签和分组**窗格。这些记录在**查看联系人**屏幕上也有自己专门的选项卡，因此我们将结合其各自的选项卡来了解它们提供的功能。

**标签和分组**选项卡提供了两种强大的机制来对联系人进行分类和组织。但是，这两种机制中哪一种最适合特定需求，并不总是显而易见的。

### 标签

标签提供了一种跨所有联系人类型的层级分类机制。它们是一种*分类*机制——您使用标签来记录有关联系人性质的信息，例如他们提供的服务类型、拥有的技能、感兴趣的领域或其他此类信息。它们可以是分层的；您可以构建带有类别和子类别的标签结构。在联系人摘要编辑屏幕上查看和选择标签时，它们会以简单的列表形式出现，并带有缩进以可视化层次结构。在专用选项卡中查看它们会显示类似的结构，但使用可展开/折叠的树形工具来更好地揭示层次结构，并允许您快速浏览该结构。

标签应用于所有联系人类型，这意味着您不能创建特定于单一联系人类型（例如组织）的标签集。请记住这一点，因为如果您可用的选项需要特定于联系人类型或子类型，您可能需要考虑其他分类工具，例如分组或自定义字段。标签也可以创建并应用于联系人、活动和案例；在创建标签时，您可以选择标签的用途。

除了层级标签结构之外，CiviCRM 还提供了创建自由格式标签集的工具。此标签功能与层级树在以下几个方面有所不同：

-   层级树显示所有可用选项的完整列表，您可以通过复选框进行浏览和选择。
-   标签树中可用的标签必须使用**管理标签**工具创建，然后您才能在联系人记录中浏览和选择它们。
-   标签集提供一个文本框，您可以在将标签分配给联系人/活动/案例时创建标签，或者用于搜索现有标签并分配。当您在标签集字段中开始输入时，CiviCRM 将动态搜索与现有标签的部分匹配，然后您可以选择这些标签。如果不存在现有标签，您将即时创建新标签。例如，如果我开始输入“trans”，它可能会拉出一个名为“transit”的现有标签；如果我继续输入“transp”，我可以继续创建一个名为“transportation”的新标签，依此类推。
-   自由格式的标签集标签也可以使用**管理标签**工具创建和管理；您只需将标签集视为父标签即可。通常，您不会使用标签管理工具为标签集创建新标签，因为自由格式标签的主要优点是在处理记录时能够动态创建新标签。但是，您可能会发现有时清理这些标签很有帮助，因为它们更容易变得杂乱。
-   总的来说，层级标签更适合以非常有条理的方式对记录进行分类，而标签集则在您想要快速轻松地列出关于记录的关键词或其他平面列表属性时非常有用。

如前所述，在将层级标签用于记录之前，您必须在系统中定义它们：**管理 | 选项列表 | 标签（类别）**。使用**新建标签**按钮创建新标签，使用**编辑/删除**链接管理现有标签。请注意，虽然管理页面按字母顺序列出所有标签，但**父 ID**列将帮助您跟踪结构的层次。

> FPAGM 使用标签来跟踪联系实体的服务类型和性质。这包括几个子类别，以进一步细化所收集的关于联系人的信息。他们定义了一个单一的“关键词”标签集，以在层级结构之外进一步对记录进行分类。

![](img/76a9a15ee6002f74a917dd7fbdfc8b1c_164_0.png)

联系人记录中的**标签**选项卡提供了最用户友好的标签选择界面。虽然您也可以选择使用**联系人编辑**屏幕来管理联系人的标签，但由于页面上的其他元素，该界面会受到更多限制。如果您的系统中有很长的层级标签列表，这一点尤其明显。**标签**选项卡以更具视觉吸引力的方式呈现层级表单，并允许您动态展开/折叠父标签以查看子选项。

![](img/76a9a15ee6002f74a917dd7fbdfc8b1c_164_1.png)

为了帮助您轻松了解联系人如何被打上标签，CiviCRM 在**摘要**屏幕上提供了所有已选标签的逗号分隔列表，如下面的截图所示：

![](img/76a9a15ee6002f74a917dd7fbdfc8b1c_165_0.png)

### 分组

分组只是记录的集合，可以静态或动态定义。静态分组有时被称为常规分组或简称为分组，而基于动态条件的组则称为智能分组。

将静态分组想象为一个容器，您可以手动将记录放入其中。没有特定标准来决定哪些人成为静态分组的一部分——他们是根据自己或您的偏好被添加或移除的。例如，您可以创建一个“董事会”分组，并将系统中当前为该组成员的九个联系人添加进去。静态分组另一个常见用途是选择加入邮件列表。当人们访问您的网站时，您可以提供注册接收定期电子邮件简报的选项。注册表单（和取消订阅工具）将使访问者能够自行添加或移除他们自己的简报分组。

相比之下，智能分组是根据您定义的条件生成的，因此本质上是动态的。例如，您可能有一个分组，检索特定区域内的所有联系人，而您定义的条件是特定的城市、州或邮政编码邻近范围。智能分组存储的是条件，而不是某个时间点的静态联系人列表，因此它们在您的数据库中充当已保存的查询。如果您稍后返回查看智能分组中的联系人，CiviCRM 将拉取一份符合所定义条件的最新联系人列表。

虽然您通常会创建一个静态分组，然后将记录添加到这个容器中，但智能分组是从搜索结果生成的。在运行联系人搜索后，您可以对结果执行某些操作，包括将其保存到智能分组中，以便日后轻松快速地检索。稍后检索已保存的智能分组时，您将有机会调整条件并更新设置。智能分组还具有手动添加和删除联系人的灵活性。因此，可以将不符合条件的人添加进来，也可以将符合条件的人排除在外。





## 使用分组

很高兴你拥有将联系人收集并组织到分组中的工具，但你如何在系统中实际使用它们呢？以下是分组最常见的用途：

- 静态分组对于你希望隔离并重复使用但不符合特定条件的记录组很有用，例如董事会和邮件列表示例。静态分组也可以在导入记录时创建，这可能是一种跟踪记录来源的有用方法。例如，如果你从第三方组织收到一份潜在捐赠者名单，你可能会导入它们并将它们添加到带有组织名称和导入日期的分组中。

- 智能分组通常会为你定期进行的保存搜索而创建。为什么要为你定期审查的记录集在高级搜索中反复选择多个条件选项呢？运行你的搜索，将结果保存到智能分组中，每当你需要检索记录时，你获取的就是符合你定义条件的准确、实时记录列表。

- Drupal 和独立实现可能会将分组用于访问控制列表（ACL）。CiviCRM 提供了大量精细的控制来限制谁可以查看、编辑、删除或访问系统中的某些功能。这些部分通过 ACL 类型的分组来控制。请注意，由于 CMS 的限制，Joomla! 实现目前尚不提供 ACL 功能，尽管预计在 Joomla! 1.6 发布时未来会开发此功能。

- 在定义分组时，你可以将其指定为邮件列表，这意味着它将可供 CiviCRM 中的群发邮件工具使用。批量邮件流程在很大程度上依赖于分组来生成收件人列表，提供根据联系人的分组状态包含或排除联系人的能力。

- 配置文件，即可向网站访客公开为搜索/列表表单、数据收集/编辑表单或只读页面的字段集合，可以根据分组状态限制其显示的记录。例如，你可以在网站上创建一个搜索和列表配置文件作为会员目录。然而，你希望确保只有状态良好的会员才属于搜索结果，因此你首先创建一个智能分组，其条件为会员状态 = 当前，以便动态检索你的会员。在配置配置文件时，你将搜索结果限制为该分组，从而创建你的在线会员目录。

联系人记录中的关系标签列出了所有活动（当前）和非活动关系。非活动关系可能发生在现有关系被手动禁用时（例如，如果公司的员工休假或被解雇，并且你手动将关系标记为禁用），或为关系记录结束日期时（例如，你创建董事会成员关系并根据两年任期记录开始和结束日期）。默认情况下，假设关系没有设定的结束日期。

输入结束日期或禁用关系而不是删除它们，提供了一种跟踪联系人在各种身份中的参与历史的有用方法。例如，通过查看当前和非活动的 **Employee of** 关系，可能有助于了解个人的就业历史。你可能会使用此信息来识别主要捐赠者的影响者，或找到可能促进潜在战略合作伙伴关系的社交网络。

在联系人记录中处理关系之前，你应查看 CiviCRM 首次设置时创建的现有关系类型，并确定是否需要创建其他类型。

为此，请前往 **Administer | Option Lists | Relationship Types.**

![](img/76a9a15ee6002f74a917dd7fbdfc8b1c_171_0.png)

| Relationship A to B | Relationship B to A | Contact Type A | Contact Type B |
|---|---|---|---|
| Case Coordinator is | Case Coordinator | Individual | Individual |
| Case Manager is | Case Manager | All Contacts | All Contacts |
| Child of | Parent of | Individual | Individual |
| Employee of | Employer of | Individual | Organization |
| Head of Household for | Head of Household is | Individual | Household |
| Household Member of | Household Member is | Individual | Household |
| Sibling of | Sibling of | Individual | Individual |
| Spouse of | Spouse of | Individual | Individual |
| Supervised by | Supervisor | Individual | Individual |
| Support Staff is | Support Staff | All Contacts | All Contacts |
| Volunteer for | Volunteer is | Individual | Organization |

此表列出了每个方向的关系标签（从 **A to B** 和 **B to A**），以及连接每一侧允许的联系人类型的限制。

如果此结构令人困惑，请在查看和创建关系时使用以下语句，将 A 和 B 替换为你的标签：

- [Contact Type A] is a/an [Relationship A to B] a/an [Contact Type B]
- [Contact Type B] is a/an [Relationship B to A] a/an [Contact Type A]

同时，查看一个非常清晰的现有关系类型，例如雇主/雇员，以了解标签文本的使用方式也可能很有用。使用上述语句与雇主/雇员关系，我们将看到：

- [an Individual] is an [Employee of] an [Organization]
- [an Organization] is an [Employer of] an [Individual]

请注意，如果你在创建关系时选择了其中一个主要联系人类型（Individual/Organization/Household），该关系将可用于所有相关子类型。然而，如果你为关系的一侧选择了子类型，它将仅可用于该特定子类型。

## 添加关系

一旦你配置了关系类型，你就可以开始在联系人之间建立连接。导航到联系人记录的 **Relationship** 标签以查看、创建和编辑其连接。

![](img/76a9a15ee6002f74a917dd7fbdfc8b1c_172_0.png)

> 可以从个人记录的摘要编辑屏幕构建两种关系：
> 
> - **Employee** of 关系将在你填写当前雇主字段时构建
> - **Household Member** of 关系将在你指示个人应共享家庭记录地址时构建

关系创建过程包括两个步骤，即根据所选类型选择相关记录，并填写关系详细信息。

除了基本的开始/结束日期、描述和备注外，还有一个通过关系处理的 basic 权限功能。使用提供的两个选项，你可以允许关系的任一侧查看和修改另一侧的联系人详细信息。

这在何处发挥作用？CiviCRM 有一个特殊的联系人摘要页面，称为联系人仪表板，可以向网站用户公开。仪表板提供了用户与组织互动的快照，包括会员历史、活动注册、捐赠历史和分组成员资格。此外，仪表板可能包含为个人定义的关系列表，如果通过此表单获得权限，它将允许已登录用户编辑相关记录的详细信息。

此功能的一个常见用例是当你希望某些管理联系人（个人）能够编辑其雇主记录时。由于组织不会访问网站（人会），你需要允许某些个人有权编辑组织记录。其他示例包括允许父母编辑其子女的记录、教师编辑其学生的记录，或家庭成员编辑家庭记录。





## Activities

到目前为止，我们主要关注记录描述联系人记录的数据。然而，CRM 的强大之处在于能够跟踪并建立与联系人的持续关系。我们需要的不仅仅是一个华丽的地址簿——我们需要能够帮助讲述我们捐助者故事的工具：我们与他们进行过何种沟通，他们选择了以何种方式在财务上或通过积极参与来支持组织，他们已成为成员多久，我们如何帮助解决他们向我们提出的问题和疑虑，以及其他此类互动。

这种故事得以保存的一个关键方式是通过活动——与捐助者之间的往来通信记录，或代表捐助者采取的行动。

活动记录将以两种方式创建：由系统在与其他流程结合时自动创建，或由与记录一起工作的员工手动创建。

某些活动记录，特别是自动电子邮件，是在系统的其他部分触发与联系人通信时创建的。例如，如果会员在线续费会员资格，表单将通过电子邮件向他们发送收据，详细说明会员记录续费、付款以及作为流程一部分完成的任何其他数据字段。除了此交易创建的会员和捐款记录之外，CiviCRM 还将电子邮件收据记录为活动，为您提供发送给联系人的每一次通信的完整图景。其他自动活动示例包括：

- Contribution receipts
- Pledge receipts
- Membership status updates
- Membership renewal reminder e-mails
- Bulk e-mails

具有适当权限的用户可以手动创建某些活动记录。最常用的是电话交谈、会议或手动生成的电子邮件记录（即在系统外部生成的电子邮件）。创建活动记录时，您可以将其标记为已完成（过去通信的记录），或将其安排在未来，在这种情况下它会成为日历待办事项。此外，活动可以分配给其他用户，这为在办公室内部管理员工之间的通信提供了有用的工具，同时将分配直接链接到捐助者记录。

## Notes

不可避免地，您与捐助者的互动将包括收集他们生活中的个人细节，遇到影响记录保存方式的独特情况或环境，或有关他们与组织持续参与的一般信息。所有这些信息都可轻松存储在备注中。

通过联系人记录上的选项卡访问，备注提供了记录捐助者一般信息的简单方法。每条备注都包括主题行、描述，并在创建时加上时间戳。

![](img/76a9a15ee6002f74a917dd7fbdfc8b1c_176_0.png)

> 请注意不要将备注用作信息的大杂烩，而这些信息更适合存储在其他地方。特别是，请确保您的用户理解活动应用于与捐助者的通信记录，而备注更适合用于有关联系人的杂项信息，例如不适合现有联系人字段的个人细节。

## Search

了解并熟悉联系人记录界面固然重要，但我们也需要能够快速在系统中找到记录。我们在探索 CiviCRM 时首先查看了导航栏中的快速搜索字段。Search | Find Contacts 扩展了此功能，允许按联系人类型、组或标签进行筛选。除了这种基本搜索之外，CiviCRM 还提供了四种强大的搜索工具，以及通过报告系统提供的额外多样化功能。

### Quick search

快速搜索位于导航菜单中，最适合在您查找特定联系人时使用。输入到此字段中的文本将搜索电子邮件地址（默认）和联系人姓名（个人按姓名排序，家庭按家庭名称排序，组织按组织名称排序）。在您开始输入时，搜索将返回部分匹配的结果。您可以选择配置该工具以使用完全通配符搜索或仅末尾通配符（Administer | Configure | Global Settings | Search Settings）。

![](img/76a9a15ee6002f74a917dd7fbdfc8b1c_177_0.png)

完全通配符搜索将在姓名或电子邮件字段的任何部分查找输入的文本。例如，输入“john”将同时找到 John Smith 和 Mark Johnson。关闭通配符搜索后，搜索“john”将仅找到 Mark Johnson，因为排序姓名字段默认构造为“lastname, firstname”。完全通配符搜索在小型数据集中非常有用，因为它提供了更大的灵活性。对于大型数据库，您通常希望将其关闭，因为结果集会变得冗长且难以处理。

如果关闭了完全通配符选项，您仍可使用百分号（%）启动通配符搜索。您甚至可以在搜索词中间使用通配符值；例如，J%on 将产生 John 以及 Jones。此“%”字符与搜索字符串中出现的任何位置的任意数量字符匹配。

### Basic search

单击 Search | Find Contacts 会弹出一个表单，允许您按姓名或电子邮件搜索并按联系人类型、组和标签筛选结果。输入的筛选条件越多，匹配数越少，因为结果必须满足所有输入的筛选条件。与联系人快速搜索一样，您可以使用通配符来扩展搜索方式。

### Advanced Search

不要让“高级”这个词吓到你——它是对该工具强大功能的评论，而不是对使用 CiviCRM 所需经验的陈述。事实上，高级搜索的强大之处在于其灵活性和易用性。要访问此工具，请导航到 Search | Advanced Search。

高级搜索提供了进入系统中几乎每个字段的窗口，按其类型分组。使用分面板界面，您可以在地址字段、捐款、会员、活动、自定义字段以及您管理的任何其他类型数据之间构建搜索条件。

高级搜索工具的唯一重要限制是，跨字段的多个条件始终由 AND 运算符连接。如果您熟悉任何形式的查询构建，您将认识到使用 AND 与使用 OR 连接多个条件之间的区别。AND 连接是排他的——随着您添加更多条件，结果列表会变小，因为记录必须满足所有提供的条件，因此更多记录被排除。OR 连接是包容性的——添加条件会扩展结果列表，因为满足任何单个条件的记录都将被包含。

高级搜索中对此规则的唯一例外是作为多选选项列表呈现的字段（包括一组分组复选框）。这些将在该字段内使用 OR 连接。例如，选择多个标签将产生匹配其中任何标签的结果。

请注意，少数特殊字段会反转正常的选择逻辑。例如，选择任何隐私选项将排除满足该标准的记录，而不是包含它们（反映了这些字段的正常使用）。如果您希望检索具有特定隐私设置的联系人列表，请选中标题为 Include contacts who have these privacy option(s) 的选项以反转默认效果。

一如既往，内联帮助文本（问号图标）将为非标准或可能令人困惑的字段提供解释和示例。

![](img/76a9a15ee6002f74a917dd7fbdfc8b1c_178_0.png)

## CiviCRM Basics: Moving through the System and Working with Contacts

构建器让您可以构建多个条件集。每个集合内的条件由 AND 连接。集合之间用 OR 组合。这使您可以创建排他性（AND）条件并将其与包容性（OR）条件集连接。

搜索构建器可通过 **Search | Search Builder** 访问。

> FPAGM 计划在 Metropolis 周边地区和城市周边的郊区对食品供应商进行更多区域外展。外展将重点放在在困难的冬季月份，当食品储藏室的需求显著增加时，争取更多的食品捐赠支持承诺。
> 
> 执行董事已确定以下标准用于初始外展信函：
> 
> - All food supplier businesses in Metroville
> - Food suppliers in Metropolis with a postal code of 10255 who have donated more than $500 at one point
> - Affiliate members with a postal code of 10254

搜索构建器中的 resulting 配置将如下图所示（OR 和 AND 文本已添加以作说明）：

![](img/76a9a15ee6002f74a917dd7fbdfc8b1c_180_0.png)





正如你所见，我们的筛选条件由三组标准组成，这些标准组合在一起得出结果。只要满足其中任意一组标准（其中两组包含多个限制性标准）的食品供应商或组织都将被包含在结果中。

使用搜索构建器时需要注意以下几点：

- 搜索条件可以基于联系人（包括类型和子类型特定字段）、捐赠、会员、活动参与者、承诺、会员、案例、活动和资助字段构建。
- 以下比较运算符用于构建条件语句。运算符定义了条件的逻辑规则，决定了输入的值将如何使用所选字段进行处理。
  - `=` 表示“等于”，代表完全匹配。
  - `!=` 表示“不等于”。
  - `>` 表示“大于”，仅应与数值字段一起使用，例如捐赠金额。
  - `<` 表示“小于”，仅应与数值字段一起使用。
  - `>=` 表示“大于或等于”，仅应与数值字段一起使用。
  - `<=` 表示“小于或等于”，仅应与数值字段一起使用。
  - `IN` 用于组、标签和其他 ID 列表。值字段可能包含用逗号分隔（无空格）的 ID 列表，并用括号括起来，例如 `(1,3,9)`。要检索 ID 值，请访问 **Contacts | Manage Groups/Manage Tags** 并记下 ID 列的值。在组或标签中工作时（例如编辑模式），您也可以通过在 URL 中查找 ID 或 GID 数字来检索 ID。
  - **LIKE** 表示“等于”，但与等号不同，您可以使用通配符与 **LIKE** 创建模糊条件。
  - **IS NULL** 表示该字段为空，不包含任何值。
  - **IS NOT NULL** 表示该字段不为空。
- 若干字段需要独特的格式：
  - **State** 和 **Country** 字段可接受完整的州/国家名称（例如 **United States**），或选项值的 ID（**1228**）。
  - 日期应使用以下格式：`YYYYMMDD`。注意没有空格或符号，例如 2010 年 3 月 16 日 = `20100316`。
- **Yes/No 字段的值应分别为 1/0（Yes = 1；No = 0）**。
- 多选自定义字段应使用在创建字段时指定的选项值作为搜索条件，而不是其标签。存储在数据库中的值可能与显示给用户的标签不同。如果需要，请重新访问您的自定义数据配置，以确保您检索到正确的值。
- 一些字段，例如 **Membership Type**，需要类型 ID 值，而不是类型名称。如果您尝试运行搜索并收到输入有效值的警告消息，很可能是因为该字段需要选项 ID 号。

## 理解联系人搜索与组件搜索的区别

到目前为止我们审查过的所有搜索工具都是基于联系人的。无论您选择什么条件，搜索都会返回联系人列表。

如果我需要一份过去六个月内超过 $500 的所有捐赠清单怎么办？如果 John Doe 在该期间捐赠了三次超过 $500，我希望看到他被列出三次；他是常客这一事实在此上下文中是很重要的信息。然而，通过 **Advanced Search**（我可以在其中访问捐赠字段）或 **Search Builder**（同样可以使用捐赠字段）进行搜索将产生一份没有重复值的联系人列表。

在整个 CiviCRM 中，通常有两种“镜头”或视角来搜索和查看记录——您可以搜索联系人，也可以搜索其相关记录。我们将“相关记录”搜索称为组件搜索。这些工具可通过主 **Search** 菜单访问（在第二个选项块中指定）或在各自的主菜单项中访问（例如，**Find Participants** 位于 **Events** 下）。

![](img/76a9a15ee6002f74a917dd7fbdfc8b1c_183_0.png)

考虑到前面的示例，在 **Advanced Search** 下搜索此条件将产生每个联系人一条记录。使用 **Find Contributions** 表单进行此搜索将产生三条记录，每笔捐赠一条。

当我们使用一些组件时，我们将进一步研究这些工具。现在，重要的是要理解搜索方式的差异以及返回的结果类型。

## 自定义搜索

如果您仍然发现需要进行复杂的搜索，而上述工具无法完成，请查看 **Search | Custom Searches....** 中的自定义搜索。

自定义搜索结构是为开发人员创建的，以便构建定制化、复杂的搜索选项和结果列表，这些列表无法通过通用表单轻松实现。说实话——并非所有可能的搜索选项都能挤进单一界面，而不造成平均用户无法使用的复杂性和混乱。CiviCRM 创建了自定义搜索结构，为开发人员提供了一个界面，以满足这些特殊需求，而不是让您直接在数据库中工作。

现有的自定义搜索表单经常在改进，并且 CiviCRM 开发社区会不断添加新的表单。请理解，要实际*创建*自定义搜索表单，必须具备开发技能（以及数据库和代码的访问权限）。然而，一旦构建，具有足够权限的用户可以访问表单并运行搜索。如果您构建了自定义搜索表单，请考虑将其贡献回项目，以便纳入未来版本。

## 搜索结果操作

到目前为止，我们已经掌握了联系人记录，可以在系统中导航，现在可以尽情进行疯狂的搜索。然而，现在您已触发搜索并获得了所需记录，下一步该做什么？

有时，您会进行搜索以查找特定记录，然后导航到联系人并对其数据进行一些操作。然而，更常见的是您搜索一组记录，并希望对全部或部分结果列表执行批量操作。

CiviCRM 通过操作下拉选项实现这一点。进行搜索后，选择 All X records 单选按钮或 Selected records only（如果您想手动从结果中选择要操作的记录）。这将启用操作下拉列表。选择所需操作并单击 Go 继续。

![](img/76a9a15ee6002f74a917dd7fbdfc8b1c_185_0.png)

从选项列表中可以看出，您可以采取的操作不胜枚举。我们将简要回顾它们，按操作类型分组，而不是按字母顺序排列。

### 邮件操作

CiviCRM 提供了两个对邮寄有用的操作，两个对电子邮件有用的操作：

- Mailing Labels: 使用 Administer | Configure | Global Settings | Address Settings 中设置的邮件标签选项将联系人打印到 PDF。

### CivicCRM Basics: Moving through the System and Working with Contacts

- **Print PDF Letter for Contacts:** 使用此选项可直接从系统中生成合并信件。您需要创建一个基本模板，模仿您文字处理软件中的布局。创建后，您可以生成信件，插入占位符标记作为合并字段，将信件保存为模板，并打印为 PDF 文件。
- **Schedule/Send a Mass Mailing:** 如果 CiviMail 已启用并配置，您可以直接从搜索结果生成批量邮件。选择此选项将触发批量邮件向导，引导您完成每个步骤。
- **Send Email to Contacts:** 如果您处理的是少量记录且不需要 CiviMail 批量邮件的全部功能，您可以使用此选项简单地向联系人发送基本电子邮件。

## 组和标签操作

在找到并选择了若干联系人后，您可能希望能够方便地稍后检索或操作这些记录。

- **Add Contacts to Group:** 将所选联系人添加到组中
- **Remove Contacts from Group:** 从组中删除所选联系人
- **New Smart Group:** 将您的搜索保存为动态智能组，以便以后使用相同条件检索
- **Tag Contacts:** 向所选记录添加标签
- **Untag Contacts:** 从所选记录中删除标签

## 添加关系操作

CiviCRM 允许您从标准搜索结果列表中向联系人添加两种类型的关系：

- **Add Contacts to Household:** 使用为家庭定义的其中一种关系类型创建与所选家庭记录的关系
- **Add Contacts to Organization:** 使用为组织定义的其中一种关系类型创建与所选组织的关系

## 添加相关记录操作

对多个联系人的几个操作涉及为每个所选联系人创建相关记录：


- **为联系人记录活动：** 创建一条活动记录，并同时关联多个联系人。该选项的一个有用用途是记录邮件发送。假设您运行搜索并想要导出记录以交付给邮件处理机构。运行导出后，向相同的结果列表添加一条活动，以记录邮件的详细信息，例如“志愿者募捐信”，并附上用于邮寄的文档副本。
- **将联系人添加到活动：** 为选定的联系人注册一个活动。此操作的唯一限制是，您必须选择一组注册选项来应用于所有选定的联系人。您也无法批量记录付款详情。

## 更新联系人操作

有几种操作直接作用于联系人记录，允许您合并、更新、删除或取消删除它们：

- **通过配置文件批量更新：** 配置文件是您在系统中定义的字段集合。您可以使用配置文件同时编辑多达100条联系人记录。
- **合并联系人：** 使用选定的记录触发去重/合并工具。您将被重定向到一个页面，在那里您可以查看记录并选择将从一条记录合并到另一条记录的字段。
- **删除联系人：** 将联系人移至回收站（如果系统启用了此功能）。
- **永久删除：** 从系统中彻底移除联系人。
- **恢复联系人：** 如果您搜索了已删除（已放入回收站）的联系人，可以使用此选项批量恢复它们。
- **解除电子邮件暂停：** 移除选定记录的“暂停”电子邮件状态。

## 导出/映射操作

以下两个操作支持对联系人记录进行某些处理，而不是更改它们：

- **映射联系人：** 如果您的站点启用了地理编码，您可以创建一张地图，并将选定的记录标记在上面。

## CiviCRM 基础知识：在系统中导航和处理联系人

- **导出联系人：** 不可避免地，您需要进行搜索并将结果导出为 `.csv` 文件（逗号分隔值），以用于各种目的——发送给邮件处理机构、对数据进行计算、合并信件、通过电子邮件传送给他人等等。导出记录的过程包括两个步骤：
  - 选择导出类型——默认主要字段或您自己选择的字段。您可以保存导出映射（字段集合）以供以后重用。
  - 如果您选择了自己选择字段，将显示一个网格，您可以在其中选择联系人类型、字段和相关字段参数。在页面底部，您可以保存导出映射、更新现有映射，并将列表导出为 `.csv`。在定义字段选择时，请记住，只有字段适合搜索结果中的联系人类型时，才会导出数据。例如，如果您的搜索结果仅包含个人，则您导出中定义的任何家庭字段都将为空。还要注意，有些选项可以导出相关记录。例如，如果您想通过雇主关系获取与个人相关联的组织所在的州值，可以使用字段列表底部附近的“相关联系人信息”选项实现。
  - 导出记录时，您可以合并具有相同地址的记录，或合并家庭记录。前者将识别具有相同地址的联系人并将它们合并为单条记录。后者将查找结果列表中属于某个家庭的记录，并导出家庭记录而不是单个联系人记录。这两个选项都将有助于防止向同一地址发送多件邮寄品，从而降低邮寄成本。请注意，这些选项不会影响记录本身；它们只会影响导出文件的生成方式。

## 对同一选择后续操作

您会发现，在许多情况下，触发一个操作后，您可以返回搜索结果列表并触发后续操作。上面的记录活动示例就是一个很好的例子——在为邮件发送导出记录后，执行第二个活动操作来记录邮寄详情。

![](img/76a9a15ee6002f74a917dd7fbdfc8b1c_189_0.png)

> FPAGM 即将召开董事会会议，必须将后勤细节通知董事会成员。他们使用简单搜索检索属于现有董事会静态组的所有联系人。从操作列表中，他们选择向结果列表中的所有记录发送电子邮件。电子邮件包含地点、时间、停车详情以及议程副本作为附件。除了提醒董事会会议的直接好处外，FPAGM 还重视这样一个事实：电子邮件的记录作为已完成的活动附加到每个联系人上。这提供了与董事会沟通的历史记录，并且如果某位董事会成员声称他们没有收到提醒，这可能会有所帮助。

## 处理联系人记录

在本章中，我们已经涵盖了很多关于联系人的内容。然而，我们的重点一直放在理解收集的数据类型、在系统中导航以及使用搜索工具和结果上。让我们通过查看替代工作流程和通过去重工具维护记录来结束本章。

## 替代工作流程

我们是在 CRM、选民或联系人关系管理工具中工作。因此，通过联系人的视角来查看数据是很自然的。到目前为止，我们涵盖的几乎所有内容都是这样做的；我们要么在单个联系人记录内工作，要么进行了搜索以对一组联系人记录采取行动。

然而，有时从操作而不是联系人记录开始可能看起来更自然。CiviCRM 为此类替代工作流提供了几个选项。

![](img/76a9a15ee6002f74a917dd7fbdfc8b1c_190_0.png)

在左侧边栏中，您会看到一个**新建**按钮和一个下拉菜单。此按钮在 Joomla! 中始终可见；在 Drupal 中是一个**默认启用**的块。Drupal 用户可以通过 Drupal 的**管理 | 站点构建 | 块**工具进行配置。除了创建联系人的选项外，还有用于创建活动、贡献、电子邮件、活动注册、成员资格、承诺、分组或标签的选项。

其中许多选项也列在相应的主菜单项下（例如，**新建贡献**列在**贡献**菜单下）。

选择创建附加到联系人的记录时，表单上会有一个联系人字段，您可以在其中开始输入现有联系人姓名，或从下拉菜单中选择创建新联系人。

“即时”创建新记录会弹出一个包含几个联系人字段的窗口。此表单可通过**管理 | 自定义 | CiviCRM 配置文件**进行配置。默认情况下，它只包含几个必填字段。您可能希望扩展包含的字段，以确保系统用户向系统中输入记录时收集更多信息。

用于“即时”创建联系人的配置文件名为**新家庭**、**新个人**和**新组织**。请注意在 CiviCRM 配置文件列表页面中，这些被标记为**保留**。在整个 CiviCRM 中，任何标记为**保留**的记录都不能被删除，并且将用于系统中的特殊目的。建议您不要对保留记录进行任何重大修改，因为这可能会在系统中产生意想不到的后果。例如，我们建议不要重命名**新个人**配置文件，然后将其用作联系人的注册表单。

## 消除重复

数据为王。如果处理的数据不正确、混乱或过时，即使是最先进的 CRM 工具也几乎毫无用处。您的系统将根据垃圾进/垃圾出的原则而生或死。

您可以采取哪些步骤来提高数据的质量和可靠性？一个重要的方法是减少记录重复。

记录重复是不可避免的现实，尤其是一旦您开始在前台暴露表单，并邀请选民和网站访问者提交他们自己的数据。幸运的是，CiviCRM 有几种处理记录重复的方法。

## 处理重复的策略

我们将使用三种策略来对抗重复：

- **政策和程序：** 是的，第一种策略是人力资源和管理策略。任何在 CiviCRM 中作为管理员工作的人都必须理解减少重复的重要性。您可以通过强制执行一项政策来开始：每次用户打算处理联系人记录时，他们首先搜索该联系人。简单，对吧？有多少次人们进入系统后立即开始创建新记录？CiviCRM 的自动填充搜索工具将有很大帮助，但只有被使用时才会对您有益。
- **内联去重：** 在创建或编辑联系人记录时，CiviCRM 会进行查找，看是否与现有记录存在潜在匹配。如果存在，您可以选择访问潜在重复记录或继续保存当前记录。CiviCRM 使用去重规则（在下面一点中讨论）来定义什么可能构成匹配。默认的模糊规则用于内联去重。您也可以使用**检查匹配联系人**按钮触发重复检查。





FPAGM 最近举办了他们的年度会议，吸引了来自该州的 350 多人，以了解食品储藏室管理的最佳实践，并听取全国各地正在开展的其他无家可归者行动倡议。大多数参与者是在线注册的。虽然这大大减少了工作人员所需的数据录入工作量，但由于许多参与者在填写表单时不够仔细，系统中确实产生了重复记录。

该协会现有的大部分联系人记录（会议之前）没有电子邮件地址，因此默认的匹配规则对于他们的需求来说过于严格。他们决定创建一条新的个人模糊匹配规则，以匹配名字和姓氏。

![](img/76a9a15ee6002f74a917dd7fbdfc8b1c_193_0.png)

## 查找重复项

匹配规则配置表单由三个可配置的列组成：字段、长度和权重。

- **字段**：指示要作为潜在匹配进行测试的数据。
- **长度**：指示应测量字段值中的多少个字符。值越低，匹配结果越多，因为如果使用较短的值，更多记录可能会找到匹配。例如，姓氏字段中长度为 4 可以匹配 Johnson 和 Johnston，而长度为 6 则无法匹配（Johnso != Johnst）。在前面的示例中，我们希望进行相当模糊的匹配，因此我们定义了一个较短的长度。将该字段留空将要求对整个字段值进行匹配。
- **权重**：用于计算匹配的可能性。使用上述规则，名字（10）和姓氏（10）都匹配会产生总分 20，达到该列底部的阈值。上面的示例非常简单；我们只是要求所有字段都匹配，系统才会将联系人视为潜在匹配。考虑一个替代配置：
  - 名字 (10)、姓氏 (10)、电子邮件 (5)、城市 (5)，总阈值 = 25
  - 在此配置中，匹配名字、姓氏和电子邮件，或者匹配名字、姓氏和城市，都能达到阈值并返回潜在匹配。

您需要仔细考虑去重规则的设置。过于严格的规则可能会导致漏掉一些有效的重复项，而过于模糊的规则会产生冗长的结果列表，不便于处理。尤其要检查默认的**严格**规则，因为这些规则会影响在无人工干预环境（在线表单和导入）中的去重/合并。

配置好规则后，单击**保存**返回规则列表，再单击以使用该规则。默认情况下，规则将针对整个数据库运行（使用该规则适用的联系人类型）。但是，您也可以选择将处理过程限制为一个群组。如果您的数据库非常大，并且希望按较小的目标记录块来处理去重，这将很有帮助。

![](img/76a9a15ee6002f74a917dd7fbdfc8b1c_194_0.png)

## 合并重复项

触发（运行）规则后，CiviCRM 会提供潜在重复联系人的列表。列表中会显示两个潜在匹配项、总权重（权重较高的匹配项排在前面），以及一个用于合并记录的链接。

合并功能才是真正体现这款工具强大之处的地方。两条记录会显示在一个网格中，由您决定哪些字段将从左侧的重复记录移动到右侧的主联系人中。这样，您可以精确选择要合并的字段。该工具还会合并任何相关记录，例如活动、贡献、会员资格等。这一点很重要，因为您不希望这些记录中包含的历史数据因合并过程而丢失。

## 合并重复项说明

碳化硅陶瓷制品具有优异的高温力学性能、热导率、抗热震性、抗氧化性能、耐磨损性能以及化学稳定性，特别适合在 1200°C 以上氧化性或腐蚀性气氛中作为结构部件长期使用。在光伏多晶硅生产领域，碳化硅陶瓷是目前多晶硅氢化炉、还原炉用高品质石墨部件(如钟罩、底盘、加热器等)的最佳替代品。

我公司生产的碳化硅陶瓷制品根据使用温度区间和工况环境要求，分为常压烧结碳化硅陶瓷和反应烧结碳化硅陶瓷两大类，产品具有以下主要特征:

- **使用温度高**：最高可达 1600°C(常压烧结碳化硅)
- **热导率高**：最高可达 125 W/mK(常压烧结碳化硅)
- **耐酸碱腐蚀**：化学惰性，耐酸碱侵蚀
- **纯度高**：常压烧结碳化硅纯度可达 99% 以上，避免对多晶硅的污染
- **热稳定性好**：抗热震性能优异，不易开裂
- **使用寿命长**：显著延长设备检修周期，降低运行成本

碳化硅陶瓷材料与石墨材料主要性能对比

| 性能指标 | 单位 | 常压烧结碳化硅 | 反应烧结碳化硅 | 高纯石墨 |
| :--- | :--- | :--- | :--- | :--- |
| 体积密度 | g/cm³ | 3.10 | 3.02 | 1.85 |
| 最高使用温度 | °C | 1600 | 1380 | 2500 |
| 热导率(20°C) | W/mK | 120 | 125 | 90-130 |
| 热膨胀系数(20-1000°C) | 10⁻⁶/K | 4.5 | 4.3 | 2.0-3.5 |
| 抗弯强度(20°C) | MPa | 380 | 250 | 40-60 |
| 抗弯强度(1200°C) | MPa | 400 | 280 | 20-30 |
| 弹性模量 | GPa | 410 | 330 | 8-12 |
| 硬度 | GPa | 27.5 | 25 | 1-2(肖氏) |
| 灰分含量 | ppm | <50 | <500 | <500 |

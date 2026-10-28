

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


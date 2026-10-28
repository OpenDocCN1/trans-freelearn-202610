
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

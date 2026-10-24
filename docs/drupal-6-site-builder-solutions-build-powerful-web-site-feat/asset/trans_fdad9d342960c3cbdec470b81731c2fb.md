

当你准备好从备份恢复网站时，你只需反向执行该过程。

1. 你应始终导入到一个空白数据库中，以避免与现有数据发生冲突。你可以删除或清空现有数据库中的所有表，或者创建一个新数据库来导入数据。
2. 清理完数据库后，在 phpMyAdmin 中选择 Import 选项卡。

![](img/b332d58f89551dfc3ae9a7cf47e6a08f_295_0.png)

3. 现在导航到你之前导出的文件，然后点击 Go 开始导入。你可能需要在导入数据之前删除数据库中的所有表，具体取决于你在导出数据时选择了哪些选项。
4. 要重新加载文件，只需打开你的 FTP 客户端，选择创建备份时所使用的相同目录，然后上传文件，而不是下载它们。

## 自动网站备份

Good Eatin' Goal: 备份网站以便轻松恢复。

Additional modules needed: Backup and Migrate (http://drupal.org/project/backup_migrate)。

### 基本步骤

虽然你可以手动备份文件和数据库，但此过程可能耗时且容易出错。幸运的是，Backup and Migrate 模块使此过程变得更容易，并优化备份以排除不必要的数据。

1. 首先下载并安装 Backup and Migrate 模块。
2. 你现在可以通过从 Administer 菜单中选择 Content management 然后 Backup and Migrate 来备份你的数据。
3. Backup and Migrate 模块允许你完全自定义创建的备份文件。你可以控制备份中包含哪些表，以及是否备份表中的数据。默认情况下，Backup and Migrate 模块不会备份缓存信息、会话信息或 watchdog 信息，因为这些表中的数据是临时的，可以轻松重新创建。

![](img/b332d58f89551dfc3ae9a7cf47e6a08f_296_0.png)

![](img/b332d58f89551dfc3ae9a7cf47e6a08f_296_1.png)

4. 你还可以从多种其他选项中进行选择，这些选项控制结果文件的命名方式、压缩方式以及保存位置。

![](img/b332d58f89551dfc3ae9a7cf47e6a08f_297_0.png)

5. 设置好选项后，点击 Backup Database 开始备份过程。如果你选择了 Download 选项，文件将被发送到你的计算机以便存储。如果你选择 Save to Files Directory 选项，备份文件将保存在服务器上，以便你稍后可以直接从服务器或使用 Saved Backups 选项卡下载。

![](img/b332d58f89551dfc3ae9a7cf47e6a08f_297_1.png)

6. 如果你希望 Backup and Migrate 模块定期自动备份你的数据库，你可以通过点击 Backup Schedule 选项卡按指定间隔安排备份，如图所示：

![](img/b332d58f89551dfc3ae9a7cf47e6a08f_298_0.png)

请注意，Backup and Migrate 模块创建的备份不包括网站文件，因此你仍需独立备份这些文件。你可以通过仅备份用户可以上传的文件来最小化备份文件大小。这些文件通常存储在 files 目录中。备份文件的过程与手动备份部分中使用的过程相同。

## 从备份恢复网站

Good Eatin' Goal: 从 Backup and Migrate 模块创建的备份文件恢复信息。

Additional modules needed: Backup and Migrate (http://drupal.org/project/backup_migrate)。

### 基本步骤

恢复由 Backup and Migrate 模块创建的备份是一个简单的过程。

1. 通过从 Administer 菜单中选择 Content management 然后 Backup and Migrate 导航到 Backup and Migrate 管理器。

2. 接下来，点击 Restore/Import DB 选项卡。

![](img/b332d58f89551dfc3ae9a7cf47e6a08f_299_0.png)

3. 导航到你的备份文件的位置。

4. 选择备份文件后，点击 Restore Database 开始恢复过程。请仔细阅读所有显示的警告，并在在生产站点上运行之前先在站点的测试安装上测试导入。如果你确定要继续导入，请同意确认并点击 restore。

![](img/b332d58f89551dfc3ae9a7cf47e6a08f_299_1.png)

5. 如果服务器文件系统未完全更新，你可能还需要导入任何已保存的文件。我们之前在手动备份部分讨论过这一点。

## 网站优化

如果你的网站有中等到大量的访客，你应该为最佳性能调整你的网站。这将确保所有访客都有良好的体验，而不会因加载页面或访问网站的延迟而感到沮丧。

我们将讨论优化后端数据库和向访客显示信息的前端网站的方法。

### 优化数据库表

*Good Eatin' Goal:* 优化存储站点内容的数据库，以确保最佳性能。

*Additional modules needed:* DB Maintenance (http://drupal.org/project/db_maintenance)。

#### 基本步骤

当数据库像 Drupal 数据库一样频繁使用时，随着时间的推移它可能会变得不够优化，因为在每个表中插入和删除大量记录会导致数据库出现浪费空间的情况。所有数据库都允许你运行优化过程，该过程会删除未使用的空间并重建索引以使信息搜索更快。

**DB Maintenance** 模块可以在 cron 运行期间自动为你执行优化过程。

1. 首先下载并安装 **DB Maintenance** 模块。
2. 你可以通过从 **Administer** 菜单中选择 **Site configuration** 然后 **DB maintenance** 来编辑 **DB Maintenance** 模块的设置。
3. 设置允许你控制多个选项，包括：
   - a) 是否在每次优化时写入日志。

![](img/b332d58f89551dfc3ae9a7cf47e6a08f_300_0.png)

   - b) 表优化的频率，选项从每小时到双月，或每次 cron 运行。

![](img/b332d58f89551dfc3ae9a7cf47e6a08f_301_0.png)

   - c) 最后，你可以控制优化哪些表。这允许你不对不经常更改的表进行优化，从而使维护任务更快完成。

配置好设置后，你只需保存设置，DB maintenance 模块将在指定的间隔自动执行优化。

### 使用缓存提高性能

Good Eatin' Goal: 使用缓存提高内容显示的性能。

Additional modules needed: None.

#### 基本步骤

由于动态生成的页面，例如使用 Drupal 创建的页面，生成起来可能耗时且资源密集，Drupal 使用复杂的缓存机制来预加载内容部分以供用户显示。

要设置 Drupal 缓存，请执行以下步骤：

1. 通过从 Administer 菜单中选择 Site configuration 然后 Performance 来访问 Drupal 缓存的设置。

2. 设置的第一部分与 Page cache 有关，该缓存用于缓存整个页面数据。Page cache 系统仅为匿名用户缓存页面，以防止一个用户的个人数据被其他用户看到，并且因为只有在为匿名用户提供缓存内容时才能更简单地确保页面被成功缓存。

#### Page cache

启用页面缓存将提供显著的性能提升。Drupal 可以存储并发送匿名用户请求的压缩缓存页面。通过缓存网页，Drupal 无需每次查看时都重建页面。

##### Caching mode:

- Disabled
- Normal (recommended for production sites, no side effects)
- Aggressive (experts only, possible side effects)

正常缓存模式适用于大多数站点且不会产生任何副作用。激进缓存模式会导致 Drupal 在提供缓存页面时跳过已启用模块的加载（boot）和卸载（exit）。这会带来额外的性能提升，但可能导致不必要的副作用。

以下已启用模块与激进模式缓存不兼容，将无法正常运行：poormanscron, uc_cart, uc_store。

大多数站点应启用 Normal 缓存，因为这可以在没有任何不良副作用的情况下显著改善访客的加载时间。

3. 如果你选择使用 Aggressive site caching，你需要了解可能出现问题的模块。Drupal 会为你识别这些模块并以红色突出显示。只有在没有潜在不兼容性时，或模块维护者声明潜在不兼容的模块可与 Aggressive caching 一起使用时，才应启用 Aggressive 缓存。一些模块由于在页面正常加载时运行的初始化或清理代码，而在激进缓存时不运行，因此与 Aggressive 模式不兼容。

4. 如果你的站点异常繁忙，你可能希望强制执行最小缓存生命周期，即 Drupal 在重建缓存页面之前等待的最短时间。如果你站点更新不频繁，你可以通过将此数字增加到与更新计划相匹配来获得性能提升。

##### Minimum cache lifetime:

5 min

在高流量站点上，可能需要强制执行最小缓存生命周期。最小缓存生命周期是在缓存被清空并重新创建之前经过的最短时间，并适用于页面和块缓存。更大的最小缓存生命周期提供更好的性能，但用户将更长时间看不到新内容。


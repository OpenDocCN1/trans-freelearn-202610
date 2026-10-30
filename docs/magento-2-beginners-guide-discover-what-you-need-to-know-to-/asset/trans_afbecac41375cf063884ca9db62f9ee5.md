

- **结账流程：** 结账流程（所有步骤合计）在 Magento Enterprise Edition 2 中，访客结账比 Magento Enterprise Edition 1.14.2 快 48%，客户结账快 36%：

    ![](img/de3034a92496a32462134266292a51dd_399_0.png)

现在让我们看看如何提高 Magento 商店的性能：

- **服务器配置：** 尽管 Magento 2 与 PHP 5.6 兼容，但建议使用 PHP 7 搭配 Varnish 以获得最佳性能效果。
- **Magento 配置 - Varnish：** 我们之前提到过 Varnish；要启用它，您应先部署并配置 Varnish。之后，您可以从管理面板的商店配置页面（**STORES | Configuration**）中启用它并打开 **ADVANCED | System** 页面。从那里，您可以选择 Varnish 作为缓存应用程序并设置配置：

    ![](img/de3034a92496a32462134266292a51dd_400_0.png)

- **Magento 配置 - Redis：** Redis 可用于替代 `Zend_Cache_Backend_File` 作为后端缓存，也可用于 PHP 会话存储。
- **Magento 配置 - 开发人员设置：** 建议使用以下开发人员设置来提高 Magento 商店的性能；您可以从 **STORES | Configuration | ADVANCED | Developer** 设置这些配置。在该页面中您将看到以下部分：

    ![](img/de3034a92496a32462134266292a51dd_401_0.png)

    设置以下配置：

    - **Grid Settings | Asynchronous indexing：** Enable
    - **CSS Settings | Minify CSS Files：** Yes
    - **JavaScript Settings | Minify JS Files：** Yes
    - **JavaScript Settings | Merge JavaScript Files：** No
    - **Template Settings | Minify HTML：** Yes
    - **JavaScript Settings | Enable JavaScript Bundling：** Yes

## Store Maintenance

- **Magento 配置：邮件：** 您可以在 **STORES | Configuration | Sales | Sales Emails** 部分将邮件设置为异步生成。如以下截图所示，您可以标记 **Asynchronous** 发送为已启用：

    ![](img/de3034a92496a32462134266292a51dd_403_0.png)

- **Magento 配置 - 平面目录：** 我们在第 10 章 *System Configuration* 中描述了类别和产品的平面目录功能。正如我们在该章中所提到的，此设置通过将所有类别和产品信息合并到一个平面表中以单次查询获取数据，从而提高商店性能。您可以在 **CATALOG | Catalog** 商店配置页面启用平面目录；这些设置在 Storefront 下为 **Use Flat Catalog Category** 和 **Use Flat Catalog Product**，如以下截图所示：

    ![](img/de3034a92496a32462134266292a51dd_404_0.png)

- **Magento 配置 - 索引：** 而不是在通过管理面板对目录进行更改时立即更新索引，您可以设置重新索引过程根据 cron 作业配置按计划执行。您可以在 **SYSTEM | Index Management** 管理面板页面进行此更改。从那里，您将看到可以使用批量操作将索引设置为按计划更新：

    ![](img/de3034a92496a32462134266292a51dd_405_0.png)

- **Magento 配置 - 生产模式：** 当服务器和 Magento 已配置好后，您应将 Magento 设置为在 **Production Mode** 下运行。在生产模式下，静态视图文件从缓存提供，而不是通过 Magento 的回退机制。此外，错误会由 Magento 记录在 `var` 文件夹中，并且不会在商店前端显示给用户。您可以通过在 Magento 2 命令行工具中运行以下命令来启用生产模式：

    ```
    php bin/magento deploy:mode:set production
    ```

    您将看到与以下截图类似的输出：

    ![](img/de3034a92496a32462134266292a51dd_406_0.png)

## Search engine optimization

除了反向链接等站外 SEO 因素外，站内 SEO 因素在您控制之下，您可以从 Magento 管理面板优化 Magento 商店的 SEO。在本节中，我们将介绍 Magento 2 开箱即用的所有 SEO 设置。

Magento 2 中的一个非常方便的功能是产品的元标签自动生成。您可以从以下管理面板部分设置该功能（**STORES | Configuration | CATALOG | Catalog | Product Fields Auto-Generation**）：

![](img/de3034a92496a32462134266292a51dd_407_0.png)

从那里，您可以使用占位符和自定义文本为目录中的所有产品自动生成 SEO 元标签。

需要明确的是，您无法为每个商店视图单独设置此功能，因为这些字段对您的 Magento 实例是全局的，并且自动生成配置的新更改不会应用于现有产品。

此外，您可以随时从产品设置中的搜索引擎优化中覆盖自动生成的值。

在以下截图中，您还可以看到您可以为产品指定 SEO 友好的 **URL key**。您也可以从同一页面通过在 **Images and Videos** 部分的 **Image Detail** 窗口中更新 **ALT Text** 来为所有产品图片配置 **ALT Text**：

![](img/de3034a92496a32462134266292a51dd_408_0.png)

如果您进入 **PRODUCTS | Categories**，您也可以为每个类别设置 URL 键、Meta Title、Meta Keywords 和 Meta Description：

![](img/de3034a92496a32462134266292a51dd_409_0.png)

最后，SEO 的两个非常重要的方面是网站的 XML 网站地图和 Robots.txt 文件。您可以从 **STORES | Configuration | Catalog | XML Sitemap** 部分为商店设置 XML 网站地图：

Robots.txt 设置可在 STORES | Configuration | General | Design | Search Engine Robots 部分找到：

![](img/de3034a92496a32462134266292a51dd_411_0.png)

在此页面中，您可以设置：

- **Default robots**：选择默认的 robots 规则：
    - INDEX, FOLLOW
    - NOINDEX, FOLLOW
    - INDEX, NOFOLLOW
    - NOINDEX, NOFOLLOW
- **Edit the custom instruction of robots.txt file**：如有需要，输入 Robots.txt 文件的自定义指令
- **Reset to Defaults**：删除自定义指令并将 Robots.txt 配置重置为默认值

## Cache and Index Management

管理 Magento 商店时需要考虑的两个方面是缓存和索引。

每次您对商店前端进行更改，例如添加新横幅或更改标题，如果启用了缓存，您必须清除它，以便客户能够在商店前端看到更新。

同样，每次您对目录进行更改时，都必须重新索引它，以便客户可以看到更新的信息。

如果您在 Magento 管理面板中打开 **SYSTEM** 选项卡，您会看到两个部分都位于 **Tools** 子菜单下：

![](img/de3034a92496a32462134266292a51dd_412_0.png)

现在，如果您进入 SYSTEM | Tools | Index Management，您将在屏幕上看到以下网格：

![](img/de3034a92496a32462134266292a51dd_414_0.png)

索引器有两种模式：

- **Update on Save**：当通过管理面板对目录进行更改时，索引数据会更新
- **Update on Schedule**：重新索引过程由 cron 作业根据其配置执行

## Upgrading Magento to a new version

在本节中，我们将回顾如何将 Magento 2 升级到最新版本。作为示例，我们将把本书中一直在审查的网站从 Magento 2.1.3 升级到 2.1.4。

在进行升级之前，您可以在页脚中看到我们使用的 Magento 版本是之前提到的那个：

![](img/de3034a92496a32462134266292a51dd_415_0.png)

要运行系统升级，您应进入管理面板中的以下部分：

**SYSTEM | Tools | Web Setup Wizard**

以下页面将显示在屏幕上：


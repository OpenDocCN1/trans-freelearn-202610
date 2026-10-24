

## 处理多个平台

Chef 的优势之一是能够将 Windows 主机与非 Windows 主机集成。不仅要开发能够同时支持 Linux 和 Windows 主机的食谱和 cookbook，还要在将其部署到基础设施之前对其进行彻底测试，这一点至关重要。让我们来看看如何在食谱中支持多个平台，以及如何使用流行的测试框架 ChefSpec 来编写测试，以测试食谱并验证特定于平台的行为。

## 在 cookbook 中声明支持

所有 Chef cookbook 都有一个 metadata.rb 文件；该文件概述了依赖项、所有权、版本数据和兼容性。在同构环境中，兼容性是一个不太重要的属性——所有主机都运行相同的操作系统。当你建模异构环境时，描述兼容性的能力就更加重要；如果没有它，你可能会尝试将仅适用于 Windows 的食谱应用到 Linux 主机上，或者反之亦然。为了指示 cookbook 支持的平台，你需要向 metadata.rb 文件添加一个或多个 supports 语句。例如，支持 Debian 和 Windows 的 cookbook 将有如下两个 supports 语句：

```
supports "windows"
supports "debian"
```

但是，如果你要支持大量不同的平台，可以始终对配置进行脚本化。例如，你可以使用类似于以下代码片段的内容：

```
%w(windows debian ubuntu redhat fedora).each |os|
  supports os
end
```

## 多平台食谱

在以下代码示例中，我们将探讨如何在单个食谱中同时在 Windows 和 Debian 系统上安装 Apache（一种流行的 Web 服务器）：

```
if platform_family? 'debian'
  package 'apache2'
elseif platform_family? 'windows'
  windows_package node['apache']['windows']['service_name'] do
    source node['apache']['windows']['msi_url']
    installer_type :msi
    # The last four options keep the service from failing
    # before the httpd.conf file is created
    options %W[
      /quiet
      INSTALLDIR="#{node['apache']['install_dir']}"
      ALLUSERS=1
      SERVERADMIN=#{node['apache']['serveradmin']}
      SERVERDOMAIN=#{node['fqdn']}
      SERVERNAME=#{node['fqdn']}
    ].join(' ')
  end
end
template node['apache']['config_file'] do
  source "httpd.conf.erb"
  action :create
  notifies :restart, "service[apache2]"
end
# The apache service
service "apache2" do
  if platform_family? 'windows'
    service_name "Apache#{node['apache']['version']}"
  end
  action [ :enable, :start ]
end
```

在此示例中，我们在主机上执行 Apache 2.x 服务的非常基本的安装。没有启用任何模块，没有虚拟主机，也没有其他内容。但是，它确实允许我们定义一个食谱，该食谱将安装 Apache，生成一个 `httpd.conf` 文件，然后启用并启动 Apache 2 服务。你会注意到这里有一点特定于平台的配置，首先是如何安装软件包，其次是如何启用服务。

由于 package 资源不支持 Windows，因此 Windows 上的软件包安装将使用 `windows_package` 资源，而 Debian 主机上则使用 package 资源。

为了使其正常工作，我们需要在安装期间应用一些配置数据；浏览食谱，我们发现需要一个类似于以下代码片段的配置哈希：

```
'config': {
    'apache': {
        'version': '2.2.48',
        'config_file': '/opt/apache2/conf/httpd.conf',
        'install_dir': '/opt/apache2',
        'serveradmin': 'admin@domain.com',
        'windows': {
            'service_name': 'Apache 2.x',
            'msi_url': 'http://some.url/apache2.msi'
        }
    }
}
```

此食谱允许我们的配置在所有节点上保持一致；我们不需要覆盖任何配置值即可在 Linux 或 Windows 主机上使其正常工作。你可能会对自己说“但是，/opt/apache2 在 Windows 上无法工作”。它可以工作，但它会被解释为 Chef 客户端当前工作目录所在驱动器上的 \opt\apache2；因此，如果你从 C: 运行 Chef，它将变成 c:\opt\apache2。通过使配置保持一致，我们不需要构建任何特殊角色来将 Windows 配置与 Linux 配置分开存储。

如果你不喜欢在 Linux 和 Windows 主机上将 Apache 安装在同一目录中，你可以轻松修改食谱，添加一些条件逻辑，如下所示：

```
apache_config_file = if platform_family? 'windows'
    node['apache']['windows']['config_file']
else
    node['apache']['linux']['config_file']
end
```

```
template apache_config_file do
    source "httpd.conf.erb"
    action :create
    notifies :restart, "service[apache2]"
end
```

在此，食谱变得稍微复杂一些，以便适应两个平台，但其好处是具有一个一致的跨平台配置规范。

或者，你可以按原样使用食谱，并创建两个角色，一个用于 Windows Apache 服务器，另一个用于 Linux Apache 服务器，每个角色都有自己的配置。apache_windows 角色可能具有以下覆盖配置：

```
'config': {
    'apache': {
        'config_file': "C:\\Apps\\Apache2\\Config\\httpd.conf",
        'install_dir': "C:\\Apps\\Apache2"
    }
}
```

相比之下，apache_linux 角色可能具有类似于以下代码片段的配置：

```
'config': {
    'apache': {
        'config_file': "/usr/local/apache2/conf/httpd.conf",
        'install_dir': "/usr/local/apache2"
    }
}
```

这种方法的影响是，现在你必须维护特定于平台的单独角色。当主机被配置或正在配置时（无论是通过控制面板还是通过 knife），你都需要记住它是 Windows 主机，因此与之关联的特定 Chef 角色。这会导致由于复杂性增加而可能出现主机配置错误。

## 降低复杂性

使用 Chef 等工具的一些关键目标如下：

- 尽量减少配置的复杂性
- 提高自动化程度
- 保证可重复性

为了实现这些目标，重要的是要确保你不会引入不必要的复杂性。虽然 Chef 为你提供了使用其多级配置覆盖创建极其复杂的配置集的机制，但这很可能会导致麻烦。正如 Eric Allman 所说的一句流行引语：

> "Unix 给你足够的绳子来吊死自己——然后再多几英尺，以确保万无一失。"

## 版本控制和源代码管理

Chef 的工作是存储 cookbook 和配置数据，然后将正确的组合提供给它所知道的主机。Cookbook 维护着自己的版本概念以及其依赖项的版本。出于这个原因，重要的是将它们视为源代码控制项目的工件，并像对待任何其他软件库一样对它们进行适当的版本控制。保持可预测的版本控制方案有助于确保与其他 cookbook 的兼容性，并减少在添加新功能或重构代码时的冲突和错误。

与任何其他软件项目一样，cookbook 版本通常以 `MAJOR.MINOR.PATCH` 的形式表示。主版本号的更改表示公共 API 不向后兼容。例如，弃用或删除公共功能或公共 API 的行为不一致，都是进行主版本更新的充分理由。相比之下，当添加新功能但现有公共 API 向后兼容时，次要版本号会递增。实现新方法或添加对不同服务管理器的支持，都是增加次要版本的充分理由。修补级别在修复错误时递增，因此这些对公共 API 应该是非常小的更改，如果有的话。有关良好版本控制实践的更多信息，请参阅 `http://semver.org`。

此外，你可能会发现自己正在从公共存储库下载 cookbook。将它们提交到内部存储库（公共或私有）可能是有意义的，这样如果你对 cookbook 进行了更改，或者公共存储库消失，你就可以使用 cookbook 代码的一个版本。

对于任何复杂的系统，无论是软件包、工具还是基础设施配置，都可以说同样的话。重要的是看看你在哪里可以简化组件，以便它们不仅现在容易理解，而且将来由其他工程师（甚至可能是你自己在凌晨 3 点试图拼命修复某个问题时）也能轻松理解。

在可能的情况下，保持配置一致并依靠食谱来区分应用程序的配置方式是个好主意。请记住，保持对分散配置片段的心理模型通常比阅读代码要复杂得多。


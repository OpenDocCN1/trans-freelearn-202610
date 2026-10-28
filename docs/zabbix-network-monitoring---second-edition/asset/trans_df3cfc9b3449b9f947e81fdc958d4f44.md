

## 可用指标

在自动创建了一些组和主机后，让我们看看它们收集了哪些数据。导航到 Monitoring | Latest data 并在 Host groups 字段中选择 Hypervisors。然后，点击 Filter：

| NAME ▲ | LAST CHECK | LAST VALUE |
|---|---|---|
| CPU (5 Items) |  |  |
| CPU cores | 2015-11-02 10:49:58 | 4 |
| CPU frequency | 2015-11-02 10:49:56 | 2.53 GHz |
| CPU model | 2015-11-02 10:49:57 | Intel(R) Xeon(R) CPU |
| CPU threads | 2015-11-02 10:49:59 | 8 |
| CPU usage | 2015-11-02 10:59:51 | 61 MHz |
| Datastore (2 Items) |  |  |
| Average read latency of the datastore datastore1 | 2015-11-02 10:59:44 | 0 |
| Average write latency of the datastore datastore1 | 2015-11-02 10:59:45 | 0 |

每个超visor 都会有更多项目。其中一些可能暂时没有数据，但稍等片刻应该就能显示出所有细节。

> Datastore 项目可能会稍后出现——它们是由 Template Virt VMware Hypervisor 中的 datastore discovery LLD 规则发现的，默认间隔为 1 小时。

现在，在 Host groups 字段中筛选超visor 虚拟机组，或在 Hosts 字段中按单个发现的虚拟机筛选：

| NAME ▲ | LAST CHECK | LAST VALUE |
|---|---|---|
| **CPU (2 Items)** | | |
| CPU usage | 2015-11-02 11:17:14 | 10 MHz |
| Number of virtual CPUs | 2015-11-02 11:17:13 | 1 |
| **Disks (4 Items)** | | |
| Average number of kilobytes read from the disk Hard disk 1 | 2015-11-02 11:16:50 | 0 Bps |
| Average number of kilobytes written to the disk Hard disk 1 | 2015-11-02 11:16:52 | 1 KBps |
| Average number of reads from the disk Hard disk 1 | 2015-11-02 11:16:51 | 0 |
| Average number of writes to the disk Hard disk 1 | 2015-11-02 11:16:53 | 0 |
| **Filesystems (4 Items)** | | |
| Free disk space on / | 2015-11-02 11:16:54 | 2.64 GB |
| Free disk space on / (percentage) | 2015-11-02 11:16:55 | 68.03 % |
| Total disk space on / | | |
| Used disk space on / | 2015-11-02 11:16:57 | 1.24 GB |

同样，应该会有更多项目，其中一些可能仍缺少值。但最终它们都应该会被填充。

> Disks、filesystems 和 interface 项目可能会稍后出现——它们是由 **Template Virt VMware Guest** 模板中的 **disk device discovery**、**mounted filesystem discovery** 和 **network device discovery** LLD 规则发现的，默认间隔为 1 小时。

一旦主机级别的所有 LLD 规则都运行完毕，我们会看到默认模板覆盖了相当多的项目。在很多情况下，这些模板甚至已经足够。有时，你可能想扩展它们。与其他默认模板一样，强烈建议先克隆模板，然后再对新模板进行修改。

此模板中的 Discover VMware VMs LLD 规则类似：它包含一个主机原型，用于所有发现的虚拟机。就像超visor 一样，UUID 用于主机名，并且它也会出现在服务器日志文件中：

![](img/f5313a804532fabacc8d07cc8607eeba_592_0.png)

在前端，我们可以同时按 **Host name** 和 **Visible name** 进行搜索。如果按主机名搜索——并且这很常见，因为我们会在日志文件中看到它——可见名称会像往常一样显示，主机名会显示在其下方并加粗，以表明它匹配了搜索：

![](img/f5313a804532fabacc8d07cc8607eeba_592_1.png)

在 **Templates** 选项卡中，我们可以看到创建的主机将被链接到 **Template Virt VMware Guest**。查看此主机原型的 **Groups** 选项卡是值得的。除了将所有发现的虚拟机添加到现有的 **Virtual machines** 组外，这里还使用了两个组原型：

![](img/f5313a804532fabacc8d07cc8607eeba_592_2.png)

正如我们在主机组页面中看到的那样，会为每个超visor 和每个集群创建一个组，其中包含该超visor 或该集群上的所有虚拟机。

## 总结默认模板交互

我们已经了解了默认的 VMware 模板集的作用，但要理解它们如何交互以及什么配置实体创建了什么内容可能会有点令人困惑。让我们尝试以示意图的形式总结它们的交互和目的。在这里，接收到所列模板的主机用粗边框表示，而各种 LLD 规则用细边框表示：

![](img/f5313a804532fabacc8d07cc8607eeba_593_0.png)

如果一个模板具有主机原型，从而导致创建更多主机，它会指向另一个粗边框的主机框，而该主机框又链接到另一个模板。

但请记住，要使这棵树开始工作，我们只需要创建一个主机并将其链接到一个模板，即 Template Virt VMware。

## 服务器操作和配置细节

我们知道 Zabbix 在收到信息后如何处理信息，但获取信息的过程本身也很复杂。那一过程本身就很有趣，但除了我们之前讨论过的 StartVMwareCollectors 之外，还有一些参数需要调整。首先，让我们看看值是如何最终进入项目的。以下示意图显示了从 VMware 开始到 Zabbix history cache 结束的数据流：

![](img/f5313a804532fabacc8d07cc8607eeba_593_1.png)

## 总结

要监控 VMware，只需要一个模板就足够了。好吧，这并不完全正确；超visor 和虚拟机的另外两个模板也必须存在，但除此之外，Zabbix 可以自动发现所有超visor 和虚拟机，就像我们在本章开头所做的那样。

我们详细研究了默认模板——它们的工作方式、交互方式以及各自提供的功能。主模板发现了一切，然后创建主机并根据需要链接超visor 和虚拟机模板。

最后，我们研究了更低级别的细节，包括数据如何通过 VMware 缓存传递、发生的频率以及我们如何调整所有这些。

在下一章中，我们将讨论一个新的 Zabbix 进程：Zabbix proxy。Zabbix proxies 是远程数据收集器，非常出色。类似于代理，它们可以以被动或主动模式运行，并支持 Zabbix server 支持的几乎所有功能，包括监控 Zabbix agents、SNMP 设备、VMware 等等。我们将设置主动和被动代理，并讨论在代理不可用时处理它的最佳方式。

## ## 19
## 使用代理监控远程位置

Zabbix server 可以使用多种不同的方法进行监控——它可以与 Zabbix agents、SNMP 设备和 IPMI 设备通信；运行命令；以及做很多其他事情。当需要监控的设备数量增加时就会出现问题——一个端点（我们的 Zabbix server）需要与大量其他设备通信，大量的连接会在 Zabbix server 和 Zabbix server 与被监控设备之间的网络组件上造成问题。

如果我们需要监控远程环境，情况会更糟——无论是分支机构、另一个数据中心还是客户现场。Zabbix agents？端口 10050 必须对所有服务器开放。SNMP？端口 161 必须对所有设备开放。很快就会变得难以管理。

解决方案是使用 Zabbix proxies。Zabbix proxy 是一个远程数据收集进程，能够使用 Zabbix server 支持的所有方法收集数据。在本章中，我们将设置一个 Zabbix proxy，用它进行数据收集，并讨论确定代理本身是否可用的最佳方法。

> > Zabbix proxies 不适用于 Windows。

为 Zabbix proxy 选择哪种数据库？如果代理将监控小型环境，SQLite 可能是一个不错的选择。使用 SQLite 作为 Zabbix server 后端是不受支持的，因为它可能会出现锁定和性能问题。在 Zabbix proxy 上问题应该会少得多。如果设置大型代理，MySQL 或 PostgreSQL 会是更好的选择。在本章中，我们将使用 SQLite 数据库的代理，因为这样设置起来非常容易。

> 如果从源代码编译，需要 SQLite 开发头文件。在大多数发行版中，它们将由名为 sqlite-devel 或类似名称的包提供。

编辑 zabbix_proxy.conf。我们将更改三个参数：

- DBName
- Hostname
- Server

将它们更改为：

```
DBName=/tmp/zabbix_proxy.db
Hostname=First proxy
Server=<Zabbix server IP address>
```

第一个参数 DBName 与 Zabbix server 相同，只是我们在这里不仅仅指定数据库名称。对于 SQLite，这里指定数据库文件的路径。虽然可以使用相对路径，但在大多数情况下启动代理会变得更加复杂，因此强烈建议使用绝对路径。我们使用 /tmp 中的文件来简化我们第一个代理的设置——无需担心文件系统权限。数据库用户名和密码呢？正如配置文件中的注释所指出的，当使用 SQLite 时，它们都被忽略了。

> 在生产系统上，建议将数据库文件放在 /tmp 以外的其他位置。在某些发行版中，/tmp 可能在重启时被清除。另一方面，出于性能原因，可以选择将数据库放在 tmpfs 卷中，从而获得一些性能提升，但每次系统重启后代理数据库都会丢失。

第二个参数 Hostname 将由代理用于向 Zabbix server 标识自己。原理与主动 agent 相同——这里指定的值必须与服务器端配置的代理名称匹配，并且区分大小写。



## 监控公共 API 的端点

- `Monitor 3 (multiple schedules, multiple public locations, and multiple environments)`：`Monitor 3` 在多个计划上运行（`Schedule 4` 和 `Schedule 5`），并关联多个公共位置（`Public Location 1` 和 `Public Location 2`）。`Monitor 3` 设置为在多个环境中测试公共 `API 5` 和公共 `API 6`。

- `Monitor 4 (multiple schedules, single location, and single environment)`：`Monitor 4` 在多个计划上运行（`Schedule 6` 和 `Schedule 7`），并关联 `Public Location 2`。`Monitor 4` 设置为在单个环境中测试公共 `API 7` 和公共 `API 8`。

- `Monitor 5 (multiple schedules, multiple locations, and single environment)`：`Monitor 5` 在多个计划上运行（`Schedule 8` 和 `Schedule 9`），并关联多个公共位置（`Public Location 2` 和 `Public Location 3`）。`Monitor 5` 设置为在单个环境中测试公共 `API 9` 和公共 `API 10`。

- `Monitor 6 (multiple schedules, single location, and the World Wide Web)`：`Monitor 6` 在多个计划上运行（`Schedule 10` 和 `Schedule 11`），并关联单个公共位置（`Public Location 3`）。`Monitor 6` 设置为测试 `API 11` 和 `API 12`，它们可通过万维网访问。

## 监控私有 API 的端点

Functional Monitoring 允许您监控可通过 Anypoint Virtual Private Cloud 访问的私有端点。私有 API 可在 Anypoint Virtual Private Cloud 内部的网络中访问。测试只能针对可在私有位置访问的私有 API 运行。可以在单个 Anypoint Virtual Private Cloud 中创建多个私有位置，以及与 Anypoint Virtual Private Cloud 关联的私有位置。

要为在 Anypoint Virtual Private Cloud 中运行的私有端点设置监控，我们需要在 Functional Monitoring 中创建一个私有位置，这将在 Anypoint Virtual Private Cloud 中创建应用程序以监控私有 API。这将默认消耗 `0.2 vCores`。此配置可根据您的需求进行更改，例如更多工作节点和工作节点大小。要创建私有位置，请在创建监控时从 `Select location` 下拉菜单中选择 `Create new location`。为您的私有位置提供 `Name` 和 `Environment` 详细信息：

![图 14.26 - 在 Functional Monitoring 中创建私有位置和监控](image/B19922_14_26.jpg)

图 14.26 - 在 Functional Monitoring 中创建私有位置和监控

创建私有位置后，它将在 Anypoint Virtual Private Cloud 内的 CloudHub 中部署应用程序，以便您可以监控私有端点：

![图 14.27 - 私有位置应用程序部署到 CloudHub](image/B19922_14_27.jpg)

图 14.27 - 私有位置应用程序部署到 CloudHub

在图 14.27 中，我们可以看到应用程序已部署到 Anypoint Virtual Private Cloud 内的 CloudHub，以便我们可以监控私有端点。它也能够监控公共端点。

下图显示了如何监控与 API、计划、私有位置和环境具有关系的私有端点：

![图 14.28 - 监控私有 API 的端点](image/B19922_14_28.jpg)

图 14.28 - 监控私有 API 的端点

以下监控已在公共位置创建，用于监控私有端点：

- `Monitor 1 (single schedule, single private location, and single environment)`：`Monitor 1` 在单个计划上运行（`Schedule 1`），关联 `Private Location 1`。`Monitor 1` 设置为在单个环境中测试私有 `API 1`。

- `Monitor 2 (multiple schedules, single private location, and single environment)`：`Monitor 2` 在多个计划上运行（`Schedule 2` 和 `Schedule 3`），关联 `Private Location 1`。`Monitor 2` 设置为在单个环境中测试私有 `API 2` 和私有 `API 3`。

- `Monitor 3 (multiple schedules, multiple private locations, and single environment)`：`Monitor 3` 在多个计划上运行（`Schedule 4` 和 `Schedule 5`），关联多个私有位置（`Private Location 1` 和 `Private Location 2`）。Monitor 3 设置为在多个环境中测试公共 `API 5` 和公共 `API 6`。

- `Monitor 4 (multiple schedules, multiple private locations, and multiple environments)`：`Monitor 4` 在多个计划上运行（`Schedule 6` 和 `Schedule 7`），关联多个私有位置（`Public Location 1` 和 `Private Location 2`）。`Monitor 4` 设置为在多个环境中测试公共 `API 6` 和公共 `API 7`。

- `Monitor 5 (multiple schedules, single private location, and single environment)`：`Monitor 5` 在多个计划上运行（`Schedule 8` 和 `Schedule 9`），关联单个私有位置（`Private Location 3`）。`Monitor 5` 设置为在单个环境中测试公共 `API 8` 和公共 `API 9`。

- `Monitor 6 (multiple schedules, single private location, and the World Wide Web)`：`Monitor 6` 在多个计划上运行（`Schedule 10` 和 `Schedule 11`），关联单个私有位置（`Public Location 3`）。`Monitor 6` 设置为测试 `API 10` 和 `API 11`，它们可通过万维网访问。

MuleSoft 提供选项，以便您可以从公共位置监控私有端点。这可以通过向 Anypoint Virtual Private Cloud 添加防火墙规则轻松实现。

在 Anypoint Virtual Private Cloud 防火墙规则中，`Type` 必须设置为 `https.port`，`Source` 可以自定义，`Port Range` 必须为 `8082`。对于自定义源，根据您的公共位置，添加表 14.2 中显示的以下 IP 地址之一：

![图 14.29 - 用于监控私有端点的 Anypoint Virtual Private Cloud 防火墙规则](image/B19922_14_29.jpg)

图 14.29 - 用于监控私有端点的 Anypoint Virtual Private Cloud 防火墙规则

使用这种方法，您无需创建私有位置即可监控私有端点。这将节省运行应用程序所需的 vCores，以便您可以从私有位置监控私有端点：

| 公共位置 | IP 地址 |
| --- | --- |
| us-east-1 | 3.82.89.58 |
| us-east-2 | 3.128.151.139 |
| eu-central-1 | 18.156.106.167 |

表 14.2 - IP 地址（公共位置）

表 14.2 显示了公共位置的各种 IP 地址。

# 为什么日志记录很重要？

日志记录对于任何应用程序都至关重要，用于事件和消息跟踪、故障排除、衡量应用程序性能以及识别应用程序中发生的错误。日志记录对于任何开发人员和运维人员快速识别和排查问题都至关重要。

MuleSoft 提供应用程序和运行时日志，以排查与应用程序和 Mule 运行时相关的问题。记录器组件可用于您的应用程序中，以进行监控和故障排除。它记录错误信息、负载和任何其他信息。此组件可在流中的任何位置使用，以记录字符串或 DataWeave 表达式，或字符串和 DataWeave 表达式的组合。

## Anypoint Monitoring 中的日志

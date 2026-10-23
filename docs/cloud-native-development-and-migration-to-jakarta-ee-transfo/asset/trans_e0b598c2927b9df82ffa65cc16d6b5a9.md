
## 设置 Grafana

Grafana 是一种用于查询和可视化数据的工具，这些数据来自我们创建的仪表板中的指标。创建仪表板的好处在于可以展示数据，让用户能够看到系统的状态。如果某些指标处于 100% 或 0%，这可能意味着系统出现了问题需要修复。与使用原始指标相比，通过仪表板更容易发现这些问题。

## 设置 docker-compose

在本节中，我们将像之前对 Prometheus 所做的那样，在容器中运行 Grafana。为此，我们需要再次修改 docker-compose 文件。我们需要为 Grafana 添加一个新服务。下面的示例展示了如何使用 Grafana 提供的最新 Grafana 镜像来添加该服务：

```
grafana:
  image: grafana/grafana:latest
  ports:
    - "3000:3000"
```

该服务将创建一个 Grafana 容器，并将容器的 3000 端口转发到宿主机的 3000 端口。

当你访问 http://localhost:3000/login 时，将看到 Grafana 的登录界面：

![](img/8948f94d0abcdb1ae94d6778da518d16_86_0.png)

图 4.2 – Grafana 登录界面

稍后我们会在容器内挂载这个路径。在 data sources 目录下，创建一个名为 `automatic.yml` 的文件。你将使用此文件来存储数据源配置。完整路径为 `grafana/provisioning/datasources/automatic.yml`。请确保将以下配置放入该 YAML 文件中。此配置告知 Grafana 以下信息：

- 数据源的名称
- 访问类型
- 可以找到资源的 URL
- 这是否为要使用的默认数据源：

```
datasources:
  - name: Prometheus
    access: proxy
    type: prometheus
    url: http://prometheus:9090
    isDefault: true
```

数据源设置完成后，我们就可以开始将仪表板导入 Grafana。

## 设置仪表板以便自动加载

如果你在 Grafana 容器内创建了一个仪表板，那么每次销毁容器并重新启动时，你的仪表板都会消失，不再存在。这并不理想，尤其是当你与同事共享项目时，他们可能完全没有仪表板。要共享仪表板并在每次创建新的 Grafana 容器时都可用，最简单的方法就是让它成为项目的一部分。将仪表板作为项目的一部分并传递给容器，这样每次创建新容器或与你共享项目的同事都能使用它。

将仪表板纳入项目并传递给 Grafana 容器需要两步。我们需要创建一个 YAML 文件来指定 Grafana 在哪里可以找到仪表板，还需要创建一个目录来存放我们将要创建的仪表板。

第一步是创建 YAML 文件，用于指定仪表板的目录。首先，你必须创建一个目录来存放 YAML 文件。我们创建了以下目录结构来存放该 YAML 文件：`grafana/provisioning/dashboards/`。在 dashboards 目录下，你需要创建一个名为 `local.yml` 的文件。你将使用此文件来存储配置。在此文件内，你需要包含以下配置：

```
apiVersion: 1
providers:
  - name: 'default'
    org_id: 1
    folder: ''
    type: 'file'
    options:
      folder: '/var/lib/grafana/dashboards'
```

配置中的第一行指定了配置的版本，此处为 1。在版本之后，我们必须指定提供程序（providers）；这会让 Grafana 知道在哪里可以找到仪表板。最重要的选项在最后一行。这一行指定了容器内仪表板将存储的目录。

下一步是创建一个仪表板，并将其存放在项目目录下的某个目录中。我们选择将 cargo-tracker 的仪表板存放在 `grafana/mydashboards/` 目录下。在 mydashboards 目录中，我们创建了一个名为 `CargoTracker-1684667357857.json` 的 JSON 文件，该文件将保存配置。下一步是让这个 JSON 文件中包含一个仪表板。我们已经为你创建好了仪表板，因此你不必再执行此操作。用于创建仪表板的完整 JSON 文件太大，无法放入本书。请查看 GitHub 上的文件：https://github.com/PacktPublishing/Cloud-Native-Development-and-Migration-to-Jakarta-EE/blob/chapter-4-fix/grafana/mydashboards/CargoTracker-1684667357857.json。

该 JSON 配置会在 Grafana 中创建一个仪表板，它将使用你在本章前面创建的 Prometheus 数据源，并将其中一些数据显示在仪表板上，稍后你就会看到。

## 展示 Grafana Docker

在创建好仪表板的配置和仪表板本身之后，我们需要将它们添加到容器中。为此，你需要在 Docker 文件中的 Grafana 服务中添加以下示例的最后两行：

```
grafana:
  image: grafana/grafana:latest
  ports:
    - "3000:3000"
  volumes:
    - ./grafana/provisioning:/etc/grafana/provisioning
    - ./grafana/mydashboards:/var/lib/grafana/dashboards
```

这两行会将你创建的目录传递给容器，使 Grafana 能够读取它们。

## 展示完整的 Docker 文件

完整的 Docker 文件如下所示。如果你按照本章的所有步骤操作，你将会修改 Cargo Tracker 应用程序，使其包含指标。创建一个 Prometheus 和 Grafana 服务，它们将从 cargo-tracker 应用程序加载配置：

```
version: "3.9"
services:
  cargo-tracker-app:
    build: .
    ports:
      - "4848:4848"
      - "8080:8080"
      - "8181:8181"
      - "9009:9009"
  prometheus:
    image: bitnami/prometheus:latest
    volumes:
      - ./prometheus/prometheus.yml:/opt/bitnami/prometheus/conf/prometheus.yml
    ports:
      - "9090:9090"
  grafana:
    image: grafana/grafana:latest
    ports:
      - "3000:3000"
    volumes:
      - ./grafana/provisioning:/etc/grafana/provisioning
      - ./grafana/mydashboards:/var/lib/grafana/dashboards
```

现在，当你运行 `docker-compose up` 时，将创建三个服务：cargo-tracker、Prometheus 和一个 Grafana 服务。当你访问 http://localhost:3000 上的 Grafana 实例，并使用 admin 作为用户名和 admin 作为密码登录时，你将看到以下仪表板。这些指标来自 Prometheus 实例，而 Prometheus 从 cargo-tracker 应用程序中拉取它们：

![](img/8948f94d0abcdb1ae94d6778da518d16_91_0.png)

图 4.4 – Grafana 中的仪表板

这些指标每隔几秒刷新一次，使你可以深入了解 `cargo-tracker` 应用程序内部，并查看其内部发生的情况。在下一章中，我们将基于本章的 Cargo Tracker 应用程序，向你展示有哪些选项可以让该应用程序更易于测试。

## 总结

在本章中，你了解了 Jakarta EE 10 最重要的变更，包括有哪些不同的 profile，以及它们何时会被使用。你还了解了 Jakarta EE 10 为你的应用程序带来的一些新特性，以及如何将 MicroProfile 与你的应用程序结合使用，使其更具弹性。

接下来，你了解了如何通过使用 Prometheus 存储健康指标，并使用 Grafana 在仪表板中显示这些指标，为你的应用程序添加监控，从而让团队更容易了解其应用程序内部发生的情况。

现在，应用程序已更新到最新版本，下一步是通过添加测试来确保应用程序持续工作。下一章将介绍如何测试你的应用程序，以确保每次更改后一切都能正常工作。

## 让应用程序更易测试

Cargo Tracker 应用程序已经有单元测试和良好的测试覆盖率。但这并不意味着你当前的项目或下一个项目也是如此。有可能你当前的项目没有单元测试，或者测试覆盖率较低。

测试覆盖率低或单元测试不足可能会带来风险。如果在迁移过程中某些东西发生了变化而无法正常工作，该怎么办？你将如何知道哪里出了问题？这时，各种不同类型的测试就派上了用场。在本章中，你将学习如何创建不同类型的测试，来测试应用程序的不同方面。

我们将涵盖以下主要主题：

- 测试对迁移的影响
- 衡量项目的代码覆盖率
- 关于**测试驱动开发**（TDD）的一点说明
- 如何创建单元测试
- 如何创建集成测试

到本章结束时，你应该了解有哪些类型的测试、何时使用它们，以及如何创建这些测试。

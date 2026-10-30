

Sentry ([https://sentry.io/](https://sentry.io/)) 是一个开源的错误监控工具替代品，正引领行业。Sentry 具有许多提供自动化的功能，例如自动分配问题规则，这意味着当代码的特定部分发生错误时，开发者会被自动分配 Sentry 问题，无需任何人工干预。我们主要使用 Rollbar，主要是因为其慷慨的免费层级和免费的 Slack 集成，而这些在大多数其他平台上是付费的。

如果团队规模更大，并且你愿意为 Sentry 付费，我更倾向于使用 Sentry。集成 Sentry 是一个直接的步骤，与我们为 Rollbar 集成所做的工作相同。

我们已经学会如何监控生产环境中发生的错误。Rollbar 和 Sentry 等工具帮助我们跟踪由于意外输入或开发者可能遗漏的边界情况而导致的所有代码级错误。此类问题仅对特定数量的请求导致 5xx 错误。但也有系统整体宕机的场景。这可能是由于基础设施问题或其他超出应用程序代码的严重问题所致。对于此类场景，我们使用 `uptime monitoring`。在下一节中，我们将探讨如何使用 uptime monitoring 来确保我们的 Django 应用获得更好的 `Service-Level Agreements` (`SLAs`)。

# Integrating uptime monitoring

你会使用一个经常宕机的应用吗？不会。当你创建应用并且用户在使用它时，你需要确保你的服务具有最大可用性。Uptime SLA 对每个服务建立用户信任至关重要。在本节中，我们将学习如何通过向自动化监控系统添加健康检查端点来监控我们的 Django 应用的正常运行时间。

## Adding a health check endpoint

`django-health-check` ([https://github.com/revsys/django-health-check](https://github.com/revsys/django-health-check)) 是一个第三方包，可以轻松添加到任何 Django 项目中，以创建可用于监控正常运行时间的端点。让我们学习如何将 `django-health-check` 集成到 Django 项目中。为此，请按照以下步骤操作：

1. 使用以下代码在终端中安装 `django-health-check` 到我们的 Django 项目：

```
pip install django-health-check
```

2. 现在在 `settings.py` 文件中将 `health_check` 添加到 Django `INSTALLED_APPS`：

```
INSTALLED_APPS = [
    # …
    'health_check',
    'health_check.db',          # Checks database connection
    'health_check.cache',       # Checks cache backend
]
```

3. 在主 `urls.py` 文件中将 `django-health-check` 端点添加到项目中：

```
urlpatterns = [
    # ...
    path(r'^ht/', include('health_check.urls')),
]
```

4. 现在，当我们在浏览器中访问 `/ht/` 端点时，我们将看到系统的健康检查。在 *Figure 14*.5* 中，我们有一个集成 `django-health-check` 的多个系统的实时系统截图：

![Figure 14.5: django-health-check output](image/Figure_14.5_B18867.jpg)

Figure 14.5: django-health-check output

现在我们有一个健康检查端点，可以在浏览器中打开以随时查看我们的服务是否正常运行。让我们添加自动化以定期监控我们的 Django 应用。有许多第三方工具，例如 Pingdom、BetterStack、FreshPing、New Relic 等，可以使用。我们将在本章中使用 `BetterStack` ([https://betterstack.com/](https://betterstack.com/))。

## Using BetterStack for uptime monitoring

在本节中，我们将学习如何使用 BetterStack 平台进行网站正常运行时间监控。BetterStack 具有慷慨的免费层级，我们将使用它通过以下步骤创建网站监控器：

1. 在 BetterStack ([https://betterstack.com/](https://betterstack.com/)) 创建新账户。

2. 一旦我们创建新账户并登录到仪表板，点击 `Create monitor` 按钮：

![Figure 14.6: Create a new uptime monitor using BetterStack](image/Figure_14.6_B18867.jpg)

Figure 14.6: Create a new uptime monitor using BetterStack

3. 现在通过在 `URL to monitor` 字段中添加 `django-health-check` 端点来创建监控器。`django-health-check` 端点在健康检查成功时将发送 `200` 响应状态码，但在任何健康检查失败时将响应 `500` 状态码。

4. 我们将利用状态码并配置 BetterStack 监控器以检查响应码是否为 `200`。如果是，则为成功。否则，健康检查将被标记为失败。在 *Figure 14*.7* 中，我们使用 *mock postman* URL (`https://<mockid>.mock.pstmn.io/ht`) 作为示例，并配置了 `Alert us when` – `URL returns HTTP status other than` – `200`。

![Figure 14.7: Configure the health check URL that we want to monitor](image/Figure_14.7_B18867.jpg)

Figure 14.7: Configure the health check URL that we want to monitor

现在，每当我们的服务器出现健康检查问题时，端点将响应 `500`，这将导致正常运行时间监控失败。

5. BetterStack 官方文档提供了将 Slack 与 BetterStack 集成的最新步骤。请按照 [https://betterstack.com/docs/uptime/slack/](https://betterstack.com/docs/uptime/slack/) 的官方文档操作。

一旦我们使用 BetterStack 设置了 Slack 集成，BetterStack 将在我们的网站宕机时发送 Slack 通知。

6. BetterStack 提供了一些其他可以轻松集成的集成。一旦我们使用 BetterStack 配置了 Slack，我们现在可以在我们的 Slack 频道和其他通信媒介上看到任何事件通知。

![Figure 14.8: Slack message when the uptime monitor fails](image/Figure_14.8_B18867.jpg)

Figure 14.8: Slack message when the uptime monitor fails

我们已成功配置 Uptime 监控器以检查我们网站的任何停机时间。每当我们的网站出现停机时间时，工程师将首先被告知事件，然后他们可以开始调查问题。

### On-call process

每个团队都应该有一名值班工程师，他将随时待命以确认由正常运行时间监控工具或其他 APM 警报触发的任何事件。设置良好的值班时间表对于生产服务的更好 SLA 很重要。

在本书中，我们没有展示如何设置值班流程，因为这超出了范围，但你可以使用不同的工具来帮助管理值班时间表，例如 SquadCast、PagerDuty 等。

事件管理工具的官方文档很直接，我建议你探索这些工具以深入了解。以下是一些进一步阅读的资源：[https://www.pagerduty.com/](https://www.pagerduty.com/)、[https://www.squadcast.com/](https://www.squadcast.com/)，以及 [https://www.atlassian.com/software/opsgenie](https://www.atlassian.com/software/opsgenie)。

我们已经学习了如何监控代码级错误和任何服务停机。随着用户开始使用 Django 应用，我们将遇到不同的规模和性能瓶颈。`Application Performance Monitoring` (`APM`) 工具用于监控生产中发生的任何性能瓶颈。在下一节中，我们将学习如何将 APM 工具集成到我们的 Django 应用中。

# Integrating APM tools

假设我们在本地开发环境中开发、测试和优化我们的应用。现在，当我们将更改部署到生产环境时，一切正常，但突然在一周后，用户抱怨响应时间缓慢，恐慌模式开始。我们现在该怎么办？我们如何调试生产环境中性能下降的原因？为了解决这个问题，我们使用 APM 工具，如 New Relic。


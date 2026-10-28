

## 缓存背景

在我们深入探讨实现细节之前，先稍微了解一下缓存如何在 Web 上工作会有所帮助。如果你花时间研究这些运行机制，那么缓存就不会像你一头扎进去那样令人困惑和沮丧。

阅读并理解相关的 HTTP 规范是很有帮助的。但是，不要以为软件总是严格遵守这些 Web 标准，即使它声称如此。

首先，让我们看一下你可能通过 HTTP 流量穿越的典型网络设置。下图展示了一个 Web 应用程序常见配置的示例：

![](img/5e416ac29f1bccaad147c6829853fd77_238_0.png)

如上图所示，笔记本电脑和平板电脑用户通过缓存正向代理服务器（可能位于企业网络或 ISP 处）进行连接。移动用户直接通过互联网连接。但是，所有用户都会在到达你的基础设施之前经过 CDN。

在你的防火墙（未显示）之后，有一个设备终止 TLS 连接，在 Web 服务器之间进行负载均衡，并充当缓存反向代理。这些功能通常由不同的设备执行，但我们在这里做了简化。

你的资源副本将保存在 Web 服务器、反向代理、CDN、任何正向代理以及所有用户设备的浏览器中。控制这些资源缓存行为的最简单方法是使用带内信令，并在你的内容中添加 HTTP 头，仅在单一位置声明缓存控制元数据。

Age 头用于指示资源已在缓存中保存了多长时间（以秒为单位）。另一方面，ETag 头用于为单个对象或该对象的特定唯一版本指定标识符。

Cache-Control 头告诉缓存该资源是否可以被缓存。它可以有许多值，包括 max-age（以秒为单位）或 no-cache 和 no-store 指令。no-cache 和 no-store 之间令人困惑但微妙的区别在于，no-cache 表示客户端在使用资源之前应与服务器确认，而 no-store 表示根本不应该缓存该资源。为了防止缓存，你通常应该使用 no-store。

ASP.NET Core 的 ResponseCache 操作特性设置 Cache-Control 头，并在第 6 章“解决网络性能问题”中介绍。但是，某些较旧的缓存可能会忽略此头。Pragma 和 Expires 是用于向后兼容的较旧头，它们执行一些现在由 Cache-Control 头处理的功能。

X-Forwarded-* 头用于向代理或负载均衡器提供有关原始连接的更多信息。这些头是非标准的，但被广泛使用，并被标准化为组合的 Forwarded 头（RFC 7239）。Via 头也提供一些代理信息，而 Front-End-Https 是一个非标准的 Microsoft 头，类似于 X-Forwarded-Proto。这些协议头对于告知你原始连接在负载均衡器剥离 HTTPS 时是否使用了 HTTPS 非常有用。

![](img/5e416ac29f1bccaad147c6829853fd77_240_0.png)

如果你在负载均衡器或代理服务器上终止 TLS 连接，并且在应用程序级别将用户重定向到 HTTPS，那么检查 Forwarded 头非常重要。如果你的 Web 服务器需要 HTTPS，但只从负载均衡器接收 HTTP，则可能会陷入无限重定向循环。理想情况下，你应该检查所有类型的头，但如果你控制代理，则可以决定使用哪些头。

有许多不同的 HTTP 头参与缓存。以下列表包括我们在这里没有涉及的头。大量的头应该让你了解到缓存可能有多么复杂：

- If-Match
- If-Modified-Since
- If-None-Match
- If-Range
- If-Unmodified-Since

我们将缓存命名为 `cache-v01`，并提供了一个要缓存的资源数组。你可能在这里有更多条目，并在函数外部定义数组，但为了清晰起见，我们在这里做了简化。

如果你的首页动态渲染实时内容，请不要缓存它。如前所述，你可能还想对资源使用缓存破坏参数。

然后我们可以添加一个 `fetch` 事件监听器来执行缓存和获取资源的魔法：

```
self.addEventListener('fetch', function (event) {
    event.respondWith(
        caches.match(event.request)
            .then(function (response) {
                if (response) return response;
                var myReq = event.request.clone();
                return fetch(myReq).then(
                    function (response) {
                        var myResp = response.clone();
                        caches.open('cache-v01')
                            .then(function (cache) {
                                cache.put(event.request, myResp);
                            });
                        return response;
                    }
                );
            })
    );
});
```

首先，我们检查请求的资源是否在缓存中，如果在，我们就返回它。使用 Promise，你可以将 `then` 函数链接在一起并逐一执行。如果由于资源不在缓存中而导致缓存未命中，我们执行 `fetch` 到我们的服务器获取资源并返回它。然后，我们通过将资源放入同一个缓存来将其添加到其他资源中。我们克隆请求和响应，因为它们是流，只能消费一次。

`fetch` 函数是 **XMLHttpRequest（XHR）** 的现代版本，用于通过网络检索数据。你不能在 service worker 内部使用同步 XHR，因为它们被设计为异步的。

## 学习缓存和消息队列

你可以使用浏览器开发者工具（F12）更详细地检查你的 service worker 和缓存。在 Resources 选项卡上，选择 Service Workers，你将看到类似以下截图的界面：

![](img/5e416ac29f1bccaad147c6829853fd77_246_0.png)

如果你选择 Cache Storage，你将看到缓存的内容，类似于下面这样：

![](img/5e416ac29f1bccaad147c6829853fd77_246_1.png)

你可以通过右键单击刷新缓存和删除项目。位于 Cache Storage 上方的 Application Cache 将显示已弃用的 AppCache 资源。当你四处导航时，你的网站页面将被添加到缓存中（如果你使用我们的演示代码，这些页面应该适合缓存，因为不会再次向服务器请求它们）。

## Web 服务器和代理服务器

从服务器的角度来看，缓存与浏览器中的客户端缓存密切相关。除了在服务器上存储资源外，你设置的头将用于控制所有位置的缓存。

你设置的 HTTP 头既被代理服务器和浏览器使用，不仅用于标准浏览，也用于从 service worker 进行获取。例如，如果 Cache-Control 头指定 no-store，那么你将无法从你的 worker 中将资源添加到缓存。

### IIS

**Internet Information Services（IIS）** 是 Microsoft 的 Web 服务器。它可用于提供 ASP.NET 应用程序中的内容，或用作代理服务器，还可以执行许多其他操作，例如 FTP。IIS 支持输出缓存，使用 OutputCache 操作特性。你还可以使用 ResponseCache 来设置正确的头，如第 6 章 *解决网络性能问题* 所述。

ASP.NET Core 2.0 现在支持在 Windows 上使用 IIS 时从 Visual Studio 2017 进行调试。你可以附加到进程并逐步执行代码，就像使用传统的 ASP.NET 项目一样。如果你计划在生产环境中使用完整版 IIS，那么在其上进行测试是一个好主意。使用 Kestrel 或 IIS Express 进行测试只能到此为止，你应该尽早做到尽可能接近真实环境。

IIS 也可以用作代理，例如，在单台机器上位于 Kestrel Web 服务器前面。但是，当为多个 Web 服务器进行缓存时，使用诸如 Varnish 之类的专用代理服务器软件可能会更好。

### Varnish

Varnish 是一个免费的反向代理服务器，运行在类似 Unix 的操作系统上，例如 **Linux** 和 **FreeBSD**。你可以使用包管理器（例如 apt 或 yum）安装它，或者使用 **Chef** 或 **Puppet** 等 DevOps 软件配置代理服务器。要配置 Varnish，你需要使用一种名为 **Varnish 配置语言（VCL）** 的**领域特定语言（DSL）**。

![](img/5e416ac29f1bccaad147c6829853fd77_248_0.png)

你可以在 http://varnish-cache.org/ 阅读更多关于 Varnish 的信息。


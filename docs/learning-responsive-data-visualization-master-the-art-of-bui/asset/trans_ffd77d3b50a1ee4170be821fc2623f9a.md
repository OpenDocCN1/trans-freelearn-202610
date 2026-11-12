

你实际上在图片中看不到，但当在应用程序中滚动时，所有连接的设备也会滚动到相同的位置。下面的截图显示了在 Android 平板电脑上运行的同一应用程序：

![](img/72559bd4713b2b89631dcca187d7e2db_211_0.png)

Android 平板电脑上的仪表板应用程序

如果你有几台备用的目标设备，使用 Browsersync 是在真实设备上快速测试可视化效果的一种非常方便的方法。

> 你可以在 https://www.browsersync.io/ 网站上找到更多有关 Browsersync 的信息。

## 远程调试响应式应用程序

如果你买不起真实设备，也不想在本地机器上运行模拟器，也可以决定从云端设备实验室访问你想要的所有设备。这结合了快速访问、低成本和丰富的设备类型：

![](img/72559bd4713b2b89631dcca187d7e2db_212_0.png)

如果你在测试 Web 应用程序，应该试试浏览器设备实验室 Browserstack。它让你直接在自己的浏览器中访问大量不同的浏览器和设备。在 Automate 模式下运行测试稍显昂贵，但在所谓的 live 模式下使用却非常出色。这让你可以通过本地浏览器运行并控制设备上的浏览器（参见下面的截图，显示 Safari 运行在 Mac 上并通过 Ubuntu 控制）。

结合前面讨论的 Browsersync，你还可以像下图所示，使用运行在 Mac 上的 Safari 浏览器实例打开本地 URL。请记住，打开本地 URL 需要一个额外的浏览器插件来通过本地网络隧道传输流量。不过，这给你提供了与之前完全相同的功能：URL 推送、点击镜像、自动重新加载等等。它与大量远程设备、操作系统和浏览器结合在一起，是一种致命的组合。

![](img/72559bd4713b2b89631dcca187d7e2db_213_0.png)

运行在 Mavericks 上的最新版 Safari

在下一个截图中，你可以看到 iPhone 6 Plus 上运行的 iOS 浏览器，它同样通过运行在 Ubuntu 上的 Chromium 浏览器进行控制。在不同设备上进行测试和调试非常简单。你甚至可以在远程设备上打开开发者工具。这对于移动设备尤其方便，因为通常你无法轻松打开开发者工具并调试应用程序。

![](img/72559bd4713b2b89631dcca187d7e2db_214_0.png)

运行在 iPhone 6 Plus 上的最新版 Safari

在 Browserstack 上进行测试和调试并不便宜，但这对每个开发者来说都是一大好处。能够直接且立即访问大量不同的浏览器和设备令人难以置信。你可以通过本地网络隧道并访问本地开发服务器，这一点简直太棒了。

> 你可以在 Browserstack 网站 https://www.browserstack.com/local-testing 上找到更多有关 Browserstack 和本地测试的信息。

## 半自动化可视化回归测试

不幸的是，由于测试和结果的歧义性与可定制性，目前尚无通用的可视化回归测试策略。要建立一个使用 GitHub 和 TravisCI 的优秀工作流，需要进行一些手动脚本编写和调整。

如果你想将其纳入工作流，我建议研究一下 PhantomCSS 和 BackstopJS（基于 Phantom 的一层抽象）。这些工具可以轻松集成到你的测试策略中，以从可视化的先前版本创建回归图像。

## 自动化测试与持续集成

如前所述，在许多情况下，我们都可以并且应该为响应式可视化运行测试。在任何可以编写自动化测试的情况下，我们都更倾向于使用自动化测试而不是手动测试。我们偏好自动化测试的原因很简单：为了使其可重复。

一旦拥有自动化测试，我们就可以在每次将更改推送到仓库时运行它们。如果它们足够快，我们甚至可以在每次对本地源文件进行更改时运行它们。这能立即反馈代码的功能性，并提供快速反馈。调试更容易，识别和查找 bug 也更容易，这会自动让你更高效，使开发更有效。

## 在本地机器上运行测试

最后，我们可以开始编写一些测试并自动运行它们。我们将从单元测试开始，之后再讨论集成测试。

## 使用 Karma 运行单元测试

作为编写和运行单元测试的第一个工具，我们需要一个可以收集所有测试、设置测试环境并在 Web 浏览器实例中启动测试的工具。这些就是所谓测试运行器的任务。在本书中，我们将使用由 Angular 团队开发的 Karma 测试运行器。

Karma 非常易于配置和使用，并且可以很好地集成到你的 Gulp 和 Grunt 构建配置中。可以通过以下命令安装：

```
npm install karma --save-dev
```

该命令还会将 Karma 添加到当前的开发依赖项中。安装 Karma 后，可以使用以下命令设置配置：

```
karma init karma.conf.js
```

在本书中，我选择了名为 Mocha 的测试框架、Require.js 和名为 PhantomJS 的浏览器来运行测试；你将得到类似下图的输出：

![](img/72559bd4713b2b89631dcca187d7e2db_216_0.png)

## 安装和配置 Karma

请确保通过运行以下命令安装了 Mocha、Chai、Require.js 和 PhantomJS：

```
npm install phantomjs mocha chai karma-chai requirejs --save-dev
```

最后，我们可以将 `chai` 和 `app.js` 添加到我们的 JavaScript 代码的配置中（你可以查看源代码），并通过命令行运行以下命令启动 Karma：

```
karma start
```

如果我们正确配置了 Karma，它将启动一个 PhantomJS 实例并运行所有测试。但是，如果我们还没有任何测试，将会出现如下图所示的 `in` 错误。太好了，现在是编写我们的第一个单元测试的时候了：

![](img/72559bd4713b2b89631dcca187d7e2db_217_0.png)

运行 Karma

我们有一个很不错的小函数，可以为我们展平嵌套数组。这是 app.spec.js 单元测试的一个完美示例；让我们编写一个简单的测试：

```javascript
var expect = chai.expect;

describe('app', function() {
  describe('#flatten()', function () {
    it('should flatten a nested array', function () {
      var nestedArray = [['foo'], ['bar']];

      var computed = flatten(nestedArray);
      var expected = ['foo', 'bar'];

      expect(computed).to.eql(expected);
    });
  });
});
```

在前面的代码中，我们将组件的所有测试都包装在 describe 块中。在这个块内部，我们将 flatten 函数的所有测试都包装在 describe 块中。最后，我们编写第一个测试来针对标准用例进行测试。

编写优秀且富有表现力的测试有很多方法和断言库；不过，我发现将所有测试都减少到绝对最小非常有用。首先，我们为 nestedArray 变量开始一个初始化阶段。接着，我们进入转换阶段，使用待测函数转换输入。我总是将输出存储在 computed 变量中。最后，我使用 expected 变量声明预期值。然后，我使用 expect(computed).to.eql(expected) 或 expect(computed).to.equal(expected)。前者测试元素级相等性，后者测试类型相等性。

如下图所示，测试通过，Karma 显示绿色的 SUCCESS 状态：

![](img/72559bd4713b2b89631dcca187d7e2db_218_0.png)

在 Karma 中通过所有测试

现在是回到测试并使其失败的好时机。如果在空数组上调用该函数会发生什么？如果数组嵌套很深呢？你已经能看出我们要往哪里走；我们的目标是破坏代码。

> 关于 Karma 测试运行器及其配置的更多信息，可以在 GitHub 上找到 https://karma-runner.github.io/。关于 Mocha 测试框架的更多信息，可以在 Mocha 网站 https://mochajs.org/ 上找到。如果你想了解更多有关 Chai 断言库的信息，请查看 http://chaijs.com/。

## 使用 Protractor 和 Selenium 进行集成测试

集成测试要求待测系统中的所有组件都能像用户正在与组件交互一样工作。因此，我们需要使用一个不同的测试运行器，它能够启动、停止和连接到浏览器，打开 URL，处理异步代码，并提取有关元素的信息。

在本书中，我们还将使用 AngularJS 团队的集成测试工具：Protractor。它基于 Webdriver 和 Selenium，因此在与不同浏览器和实例交互时非常强大。

我们可以通过在命令行中运行以下命令来安装 Protractor：

```
npm install -g protractor
```

安装 Protractor 后，我们还需要安装 Webdriver。可以通过运行以下命令来完成：

```
webdriver-manager update
```


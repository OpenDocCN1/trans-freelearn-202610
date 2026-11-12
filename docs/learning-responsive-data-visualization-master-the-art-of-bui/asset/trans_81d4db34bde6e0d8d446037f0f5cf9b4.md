

使用 Protractor 运行集成测试时，我们总是需要事先运行 Webdriver。因此可以通过以下方式启动它：

```
webdriver-manager start
```

如果一切安装正确，您将看到类似下图的输出：

![](img/72559bd4713b2b89631dcca187d7e2db_219_0.png)

后台运行的 Selenium

现在，我们可以创建一个名为 protractor.conf.js 的简单配置文件和第一个测试 app.e2e.js。查看源代码以获取正确的设置：

```
describe('dashboard', function() {
beforeEach(function() {
// Don't wait for Angular
return browser.ignoreSynchronization = true;
});

it('load choropleths', function() {
browser.get('B05166_08_Dashboard.html');

var maps = element.all(by.css('.choropleth'));

var result = maps.count();
var expected = 1;

expect(result).toEqual(expected);
});
});
```

在前面的代码中，我们再次将所有测试包装在 describe 块中。然后，我们添加一小段代码来告诉 Protractor 不要等待 AngularJS。最后，我们编写一个访问 Browsersync 开发服务器的测试。如果我们启动测试，它将运行并希望像下图一样没有错误地完成：

```
ckoeerner@koerner-ThinkPad-W550s:~/Dropbox/msc-private/Projekte/Responsive Data Visualizations with Bootstrap and D3.js/chapter09/codes$ protractor protractor-conf.js
[11:27:24] I/direct - Using the selenium server at http://localhost:4444/wd/hub
[launcher] Running 1 instances of WebDriver
Started
..
1 spec, 0 failures
Finished in 1.605 seconds
[launcher] 0 instance(s) of WebDriver still running
[launcher] chrome #01 passed
ckoeerner@koerner-ThinkPad-W550s:~/Dropbox/msc-private/Projekte/Responsive Data Visualizations with Bootstrap and D3.js/chapter09/codes$
```

Protractor 中所有测试均通过

现在是创建包含单个可视化组件以及该组件测试的集成测试文件的好时机。尝试添加滚动、悬停和点击来打破可视化。

> 可以在 Angular 网站 https://angular.github.io/protractor 上找到有关 Protractor、其配置和 API 的更多信息。

## 在云中运行您的测试

现在进入高级部分；您可以编写一个脚本，通过 TravisCI 在每次推送时运行完整的集成测试。查看 https://docs.travis-ci.com/user/languages/javascript-with-nodejs 中的步骤，以开始使用 Travis 和 JavaScript。

但是，您不需要在 Travis 的虚拟机中运行浏览器，可以添加诸如 Browserstack 之类的服务，在云中使用多种设备和分辨率运行测试。

我想提到另一个很棒的自动化设备测试服务，它是 Browserstack 的绝佳替代品，并且对开源项目免费——Sauce Labs。

> 可以在 Sauce Labs 的网站 https://saucelabs.com/ 上找到更多信息。

## 可视化的测试策略

现在，我们有足够的信息来为响应式可视化构建一个稳固的测试策略。请注意，这是基于我个人经验的一个非常主观的设置，请随意用您喜欢的层或工具替换任何部分。但是，请为您的工作流程保留总体思路。

首先，开发时我们将使用台式机上的 Chrome 浏览器。我们既可以使用普通的桌面窗口，也可以在设备模式下切换到移动视图。这取决于目标屏幕大小和分辨率。个人而言，我更喜欢在桌面 Chrome 中工作，并在移动设备上调试和测试。

开发期间，我们需要定期进行一些手动测试，以确保可视化看起来和感觉良好。在本地为不同操作系统运行模拟器很麻烦；这就是为什么我更喜欢通过 Browsersync 将真实的移动手机和平板电脑同步到当前的 Chrome 窗口。如果您无法访问真实设备，也可以在本地项目中使用 Browserstack。

对于单元测试，在开始进行更改之前，我们需要启动一个 Karma 单元运行器。它将监视源文件的更改，启动 PhantomJS 实例，并在保存文件时运行完整的测试套件。

一旦我们将提交推送到 GitHub，TravisCI 就会在本地自动运行所有单元测试，并使用 Sauce Labs 在不同的移动设备上运行集成测试。这还将为视觉回归测试创建不同的图像，并将其显示给开发人员。

最后，当所有测试都通过且开发人员接受视觉回归的不同图像时，功能或修复可以合并到 master 分支中。

## 总结

本章对不同测试策略用于测试响应式可视化组件提供了扎实的概述。在第一部分，我们讨论了测试策略、测试的成本和收益以及本地、远程、手动和自动测试之间的区别。

然后，我们将了解不同的测试策略以及如何将它们实现为可视化。我们涵盖了使用 Browsersync 和 Browserstack 进行的手动测试，以及使用 TravisCI、Karma、Protractor 和 Selenium 进行的自动化测试。最后，我们将为可视化组件构建完美的测试策略。

在下一章中，您将学习在创建 SVG 中的响应式可视化时检测和解决跨浏览器问题。

## 解决跨浏览器问题

在上一章中，您了解了在不同浏览器中测试自定义响应式可视化的所有内容。我们必须这样做，因为在不同的设备、操作系统和浏览器中，我们的应用程序的外观和功能可能会有所不同。在本章中，我们将解决这些跨浏览器问题。您将学习以下内容：

- 了解跨浏览器兼容可视化的主要挑战
- 使用 Modernizr 检测用户浏览器的功能支持
- 了解 Polyfills、Shims 和 Wrappers
- 了解 D3 的兼容性
- 实现跨浏览器响应式设计
- 了解 matchMedia polyfill
- 使用 Respond.js 对 min-width 和 max-width 进行 Polyfill
- 使用 Enquire.js 在 JavaScript 中实现媒体查询
- 了解 SVG 功能的兼容性
- 了解鼠标和触摸事件的兼容性
- 了解触摸事件的执行链
- 构建触摸友好的可视化

首先，我们将研究常见的跨浏览器兼容性问题并找到解决方法。从针对 Internet Explorer 的简单条件注释开始，我们将逐步深入到使用 Modernizr 在 JavaScript 和 CSS 中进行复杂的功能检测。您将了解解决跨浏览器问题的常见概念，例如 Polyfills、Shims 和 Wrappers。

在第二部分，您将了解跨浏览器兼容的媒体查询并理解 CSS polyfill 的问题。然后，我们将使用 Respond.js（一个 min-width max-width polyfill）和 Enquire.js（一个用于在 JavaScript 中组织媒体查询的库）来研究此问题的不同解决方案。

接下来，我们将查看不包含在 1.1 规范中的特殊 SVG 功能的支持，例如动画、滤镜和裁剪路径。

在最后一部分，您将学习如何在同时支持鼠标和触摸输入的设备上正确处理触摸和鼠标输入。我们将看到一些常见的陷阱、触摸事件，以及如何为您的应用程序同时支持触摸和鼠标事件。

## 跨浏览器兼容性问题的解决方案

开发 Web 应用程序的真正挑战不在于实现很棒的功能、动画或过渡效果，而在于为所有目标设备、操作系统和浏览器提供一流的用户体验和兼容性。在本章中，我们将尝试解决最常见的兼容性问题，并使您的可视化跨浏览器兼容。

### 针对 Internet Explorer 的条件注释

解决兼容性问题的每个 Web 开发人员工具箱中一个很棒的工具是 HTML 的条件注释。这些是可以包含有效 HTML 代码的 HTML 注释，由 Internet Explorer 解释和执行。等等，一个仅在 Internet Explorer 中可用的酷功能？嗯，事实证明，这些条件 HTML 注释在现代 Web 浏览器中不是必需的，因为这些浏览器更愿意实现适当的标准而不是专有功能。

因此，条件注释是针对 Internet Explorer 的迷茫开发人员根据浏览器版本加载资源的绝佳方式。让我们看一个简单的示例，我们仅为版本小于或等于 9 的 Internet Explorer 加载兼容性脚本：

```
<!doctype HTML>
<html>
<head>
	<title>Conditional Comments</title>
	<!--[if lte IE 9]>
	<script src="compatibility_script.js"></script>
	<![ENDIF]-->
```


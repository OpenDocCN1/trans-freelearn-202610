
最后，Docker 会输出刚刚创建的镜像 ID。让我们检查我们的镜像是否在镜像列表中，执行众所周知的 docker images 命令：

```
docker images
```

如果一切顺利，你将在镜像列表中看到你刚刚创建的镜像，如下图所示：

![](img/0c87321919fa245e22f056c67ec70003_235_0.png)

镜像已在列表中，让我们通过执行以下命令来运行它：

```
docker run -it -p8080:8080 hello-docker
```

请注意，我们包含了 -p 选项以自动将暴露的端口映射到主机的端口。正如你可能记得的，我们在 pom.xml 配置中暴露了端口号 8080。执行命令后，你将看到与之前在本地运行 java -jar 命令时相同的输出。

然而，现在它来自 Docker 容器中包含的应用，如下图所示：

![](img/0c87321919fa245e22f056c67ec70003_236_0.png)

为了验证 Docker 容器中的应用程序确实能工作，再次将你的网页浏览器指向 http://localhost:8080 地址：

![](img/0c87321919fa245e22f056c67ec70003_236_1.png)

另外，如果要手动运行 docker run，我们可以将 runtime 部分包含到项目的 pom.xml 文件中。整个 <image> 元素将类似于这样：

```
<image>
  <name>hello-docker</name>
  <build>
    <from>openjdk:8u102-jdk</from>
    <ports>
      <port>8080</port>
    </ports>
    <entryPoint>
      <exec>
        <args>java</args>
        <args>--jar</args>
        <args>/maven/hello-docker-${project.version}.jar</args>
      </exec>
    </entryPoint>
    <assembly>
      <descriptorRef>artifact</descriptorRef>
    </assembly>
  </build>
  <run>
    <ports>
      <port>8080:8080</port>
    </ports>
  </run>
</image>
```

现在你可以在一步中编译、打包、构建并运行镜像：

```
mvn clean package docker:build docker:run
```

现在我们有一个完全可用的 Spring Boot 应用被打包为 Docker 容器。它已准备好推送到 Docker Hub 或你自己的私有仓库。只要 Docker 可以运行，此应用程序将以相同的方式工作。想象一下，你为应用程序的用户节省了多少麻烦，操作系统配置、Java 设置和配置、依赖管理等等。这只是一个相当简单的示例，但它应该能让你了解 fabric8.io Docker 插件有多灵活和有用。

现在我们知道了如何打包 Java 应用程序，让我们做更多的事情。我们将打包一个示例前端应用程序。这将是一个在 node.js 上运行的 Angular.js 前端。

## 打包 Angular.js 应用程序

在本示例中，我们将创建一个在 node.js 运行时上运行的简单 Angular.js 应用程序。为了示例的清晰性，我们不会引入任何测试。正如 Spring Boot 应用程序的情况一样，我们的 Angular.js 应用程序将尽可能简单和精简。我们将了解如何将其容器化，而不是深入 Angular.js 开发。当然，如果你还没有了解 Angular.js，欢迎你去了解它；它是一项很棒的技术。在开始之前，你需要安装 node.js。你可以从 node.js 网站 https://nodejs.org/en/ 获取安装程序。

首先，让我们将应用程序的结构分为两部分。第一部分是前端，包含所有静态资源，如 JavaScript 和 HTML 文件。第二部分是 node.js 后端，实际提供前端文件并响应 HTTP 请求。以 angular-docker-example 为父目录的文件夹结构最初将类似于以下截图：

![](img/0c87321919fa245e22f056c67ec70003_238_0.png)

我们将使用 Bower 来管理第三方 JavaScript 库。Bower 使用 bower.json 文件，在其中定义需要运行的所有应用程序。目前，我们只需要一个 angular.js 依赖项。以下是我们这个小应用程序的最小 bower.json 文件：

```json
{
  "name": "hello-docker-angular ",
  "ignore": [
    "**/*",
    "node_modules",
    "bower_components"
  ],
  "dependencies": {
    "angular": "~1.5"
  }
}
```

我们希望所有依赖项都下载到我们的前端应用程序文件夹中。因此，在执行 bower install 之前，我们需要告诉 Bower 我们希望这些文件去哪里。为此，我们需要创建一个 .bowerrc 文件并将其放在项目的根目录中。该文件的 JSON 内容将包含所有将被下载的依赖项的目的地：

```
{
  "directory": "frontend/bower_components"
}
```

接下来，你首先想要安装 bower 本身，你可以在系统中全局安装它，通过执行 npm install 命令：

```
npm install -g bower
```

安装了 Bower 并放置了 .bowerrc 后，我们现在可以下载所有所需的前端依赖项。在项目的根文件夹中执行以下操作以开始下载过程：

```
bower install
```

过了一会儿，Bower 完成下载所有依赖项（在我们的例子中只是 Angular.js）。它们将存在于 frontend/bower_components 目录中。由于只有一个依赖项，bower_components 目录中只会创建一个名为 angular 的目录，如下图所示：

![](img/0c87321919fa245e22f056c67ec70003_239_0.png)

我们的 index.html 将作为应用程序前端部分的入口点，需要放在前端模块的根目录中，在我的例子中是 angular-docker-example\frontend。它将类似于以下代码：

```html
<script src="bower_components/angular/angular.min.js"></script>
<script src="hello_world_docker.controller.js"></script>
<div ng-app="HelloWorldApp">
    <div ng-controller="HelloWorldDockerController">
        Your name: <input ng-model="name" ng-keyup="sayHello()">
        <h1>{{greeting}}</h1>
    </div>
</div>
```

我们还需要向应用程序添加控制器（它将是 hello_world_docker.controller.js 文件），它将对按键抬起事件做出反应。如果你将你的名字写在文本字段中，Angular 将用你的名字向你问好：

```javascript
angular.module('HelloWorldDockerApp', [])
    .controller('HelloWorldDockerController', function($scope) {
        $scope.sayHello = function () {
            $scope.greeting = "Hello from Docker-Angular, " +
                $scope.name;
        };
    });
```

现在是测试我们应用程序的时候了，只需在你选择的网页浏览器中打开 index.html 文件并在文本字段中输入你的名字：

![](img/0c87321919fa245e22f056c67ec70003_240_0.png)

好的，它按预期工作。让我们继续进行服务器部分——Web 应用程序需要被提供服务。当然，我们的简单应用程序通过直接在网页浏览器中运行 HTML 文件来工作。如果你以后决定包含更多 Angular 功能，例如路由，直接打开将不起作用。此外，服务器通常通过响应 HTTP 请求（例如 REST 调用）来完成应用程序。从这些调用中获取的响应然后可以在静态 Angular 页面上使用。我们可以使用任何我们喜欢的编程语言，例如 Java 和 Spring Boot 以及我们之前示例中使用的 docker-maven 插件。这次，为了展示将应用程序手动打包到 Docker 容器的过程，我们将使用 node.js 运行时作为我们应用程序的后端。为了获取所需的 node.js 依赖项，我们需要创建一个包含所有所需依赖项的 package.json 文件：

```
{
  "name": "hello-world-docker",
  "main": "index.js",
  "dependencies": {
    "express": "^4.12.3",
    "bower": "1.7.9"
  }
}
```

package.json 文件与 bower.json 相同，需要放在项目的根目录中。如果文件准备好了，我们可以运行 npm install 命令来下载所需的依赖项，在我们的例子中是 Express 和 Bower。执行以下命令：

```
npm install
```

过了一会儿，node_modules 文件夹将被创建。它将包含 package.json 文件中提到的依赖项，我们的应用程序将使用它们。我们 node.js 应用程序的入口点将是一个简单的基础 HTTP Web 服务器，从 frontend 目录提供静态内容。让我们将其命名为 app.js 并用以下内容填充它：

```
'use strict';
process.env.NODE_ENV = process.env.NODE_ENV || 'development';
var express = require('express');
var http = require('http');
var path = require('path');
var app = express();
var config = {
  port: 8080,
  root: path.normalize(path.join(__dirname, '../frontend'))
}
app.use(express.static(config.root));
```

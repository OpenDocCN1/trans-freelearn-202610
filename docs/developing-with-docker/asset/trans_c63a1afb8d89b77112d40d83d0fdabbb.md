
如你在前面的 Dockerfile 中所见，它使用 EXPOSE 暴露了两个端口：标准 HTTP（80）和 SSL（443）。这些端口将可供此主机上的其他容器使用，并且——如果你在运行时暴露它们——也对外部世界可用。你可以显式地使用指定端口运行镜像：

```
docker run nginx -p 80
```

另选地，你可以告诉 Docker 我们希望所有暴露的端口一次性发布：

```
docker run nginx -P
```

良好做法是为你的应用程序使用通用、已知的端口号。例如，如果你的应用程序运行在 nginx Web 服务器上，你应该使用端口号 80 进行 EXPOSE，这样应用程序的用户就不会对一些奇特的端口号感到惊讶。当然，如果有暴露非标准端口号的特定需求，你可以自由选择，这只是良好实践。

## RUN

RUN 指令是 Dockerfile 的核心执行指令。本质上，RUN 指令会在当前镜像之上新建一层执行任意命令，然后提交结果。提交后的镜像将用作 Dockerfile 中下一条指令的基础。正如你在第 3 章 *理解镜像和容器* 中记得的那样，分层是 Docker 的核心概念。RUN 将命令作为参数并运行它以形成镜像。RUN 指令可以运行任何命令（或应用程序），这与我们之前了解的 CMD 指令类似。有一个关键的重要区别——执行时间。通过 RUN 指令提供的命令在构建时执行，而通过 CMD 指令指定的命令在通过在新创建的镜像上执行 docker run 启动容器时执行。与 CMD 不同，RUN 指令实际上用于构建镜像，通过在之前的基础上创建新层并提交。

![](img/0c87321919fa245e22f056c67ec70003_153_0.png)

CMD 是运行时指令，而 RUN 是构建时指令。

这也意味着 CMD 和 ENTRYPOINT 设置的参数可以在运行时被覆盖，因此如果你不做任何更改，启动容器后结果始终相同。而 RUN 则在构建时执行，无论你在运行时做什么，其效果都会在这里体现。

RUN 与 CMD 和 ENTRYPOINT 类似，可以有两种形式：

- `RUN <command>`: 这是 shell 形式。提供的命令及其参数将在 shell 中执行，在 Linux 上为 `/bin/sh -c`，在 Windows 上为 `cmd /S /C`。RUN 指令的默认 shell 可以使用 SHELL 命令更改。
- `RUN ["executable", "parameter1", "parameter2"]`: 这是 exec 形式。与 CMD 中的规则相同。使用此形式，你可以在不包含指定 shell 可执行文件的 base 镜像上执行命令。此形式的 RUN 不会调用命令 shell。同样，与 CMD 或 ENTRYPOINT 一样，正常的 shell 处理，例如变量替换，不会发生。

![](img/0c87321919fa245e22f056c67ec70003_154_0.png)

为了使 Dockerfile 更易读且更易维护，你可以将长或复杂的 RUN 语句拆分为多行，用反斜杠分隔。

Dockerfile 中的 RUN 命令将按其出现的顺序执行。每个 RUN 指令都会在镜像中创建一个新层。层，如你已从第 3 章 *理解镜像和容器* 中所知，被 Docker 缓存并重用。RUN 指令的缓存不会在下一次构建期间自动失效。例如，像 RUN apt-get upgrade -y 这样的指令的缓存将在下一次构建期间被重用。缓存的重要性是什么？在大多数情况下，缓存非常有帮助，可以在构建镜像时为你节省大量时间。如果 Docker 即将产生的对象在构建之间未受影响，重用主机上之前构建的缓存是极大的节省时间。它使构建新容器变得非常、非常快。这些文件结构不必再次创建并写入磁盘——对它们的引用足以定位并重用之前构建的结构。然而，有时缓存可能是危险的并提供意外的结果。缓存在构建过程中被大量使用，这可能会导致问题，当你希望 RUN 命令的更新输出进入新容器时。

如果 RUN 命令在两次构建之间没有变化，Docker 的缓存将不会失效。实际上，Docker 将重用缓存中的先前结果。当例如 RUN 命令是源代码检出，例如项目的构建第一步的 Git clone 时，这显然是有害的。

![](img/0c87321919fa245e22f056c67ec70003_155_0.png)

请注意缓存有时需要失效，否则你将用镜像构建得到意外的结果。

这就是为什么了解如何选择性地使缓存失效是好的。在 Docker 世界中，这被称为缓存破坏。

考虑以下示例。RUN 最常见的用例可能是 apt-get 的应用，它是 Ubuntu 上用于下载软件包的包管理器命令。假设我们有以下 Dockerfile，在镜像中安装 MongoDB：

```
FROM ubuntu
RUN apt-get update
RUN apt-get install -y mongodb-server
```

如果我们从此 Dockerfile 构建镜像，两个 RUN 指令的所有层都将放入层缓存中。但，过了一段时间你决定在镜像中想要 node.js 软件包，于是 Dockerfile 现在看起来像这样：

```
FROM ubuntu
RUN apt-get update
RUN apt-get install -y mongodb-server
RUN apt-get install -y nodejs
```

在第二次构建时，Docker 将通过从层缓存中获取来重用层。结果 apt-get update 将不会执行，因为将使用缓存版本。实际上，你新创建的镜像可能具有过时的 MongoDb 和 node-js 软件包版本。你在创建 RUN 指令时应始终牢记缓存概念。在我们的示例中，我们应始终将 RUN apt-get update 与 apt-get install 结合在同一个 RUN 语句中，这将只创建一个层。考虑以下示例：

```
RUN apt-get update \
&& apt-get install -y mongodb-server \
&& apt-get install -y nodejs
```

比这更好的是，你还可以使用一种称为 **版本锁定** 的技术来避免缓存问题。它无非就是为你想要安装的软件包提供特定、具体的版本：

```
RUN apt-get update \
&& apt-get install -y mongodb-server \
&& apt-get install -y nodejs=0.6.12~DFSG1-1UBUNTU1
```

![](img/0c87321919fa245e22f056c67ec70003_156_0.png)

由于缓存问题，单独在 RUN 语句中使用 `apt-get update` 不被推荐。

RUN 指令的缓存可以通过 ADD 指令或使用 `--no-cache` 标志来使失效，例如：

```
docker build --no-cache
```

## USER

USER 指令非常简单。它设置运行镜像时使用的用户名或 UID。它还影响 Dockerfile 中其后出现的任何 RUN、CMD 和 ENTRYPOINT 指令的用户。

指令的语法只是 USER <user name or UID>，例如：

```
USER tomcat
```

如果你可以在没有特权的情况下运行可执行文件，可以使用 USER 命令。Dockerfile 可以包含如下的用户和组创建指令：

```
RUN groupadd -r tomcat && useradd -r -g tomcat tomcat
```

![](img/0c87321919fa245e22f056c67ec70003_156_1.png)

频繁地在 USER 之间切换会增加结果镜像中的层数，并使 Dockerfile 变得更复杂。

## VOLUME

正如你从第 3 章 *理解镜像和容器* 中记得的那样，Docker 文件系统默认是临时性的。如果你启动一个 Docker 镜像（即运行容器），你将在层堆栈顶部得到一个读写层。你可以随意创建、修改和删除文件，然后提交层以保留更改。此外，使用 Dockerfile 将创建只读镜像。在第 4 章 *网络和持久化存储* 中我们学习了如何创建卷——从 Docker 容器存储和检索数据的好方法。我们可以在 Dockerfile 中使用 VOLUME 指令做同样的事情。

语法 couldn't be simpler，它只是 `VOLUME [ "/volumeName" ]`。

VOLUME 的参数可以是 JSON 数组，也可以是包含一个或多个参数的纯字符串，例如：

```
VOLUME ["/var/lib/tomcat8/webapps/"]
VOLUME /var/log/mongodb /var/log/tomcat
```

VOLUME 指令创建具有指定名称的挂载点，并将其标记为保存来自原生主机或其他容器的外部挂载卷。

VOLUME 命令将在容器内挂载一个目录，并将该目录内创建或编辑的任何文件存储在主机磁盘上容器文件结构之外。使用 Dockerfile 中的 VOLUME 让 Docker 知道某个目录包含永久数据。Docker 将为该数据创建卷，并且即使你删除使用它的所有容器也不会删除它。它还绕过联合文件系统，因此卷实际上是一个实际目录，在所有共享它的容器中以正确的位置挂载，要么是读写，要么是只读（如果它们是用 `--volumes-from` 选项启动的，例如）。为了理解 VOLUME，让我们看一下简单的 Dockerfile：

```
FROM ubuntu
VOLUME /var/myVolume
```

如果你现在运行容器并在 `/var/myVolume` 中保存一些文件，它们将可供其他容器共享。

基本上，VOLUME 和 `-v` 选项几乎相等。VOLUME 和 `-v` 选项之间的区别是，你可以使用 `-v` 选项动态地在启动容器时通过执行 `docker run` 将主机目录挂载到容器上。这是因为 Dockerfile 旨在可移植和共享。主机目录卷是 100% 主机依赖的，并且会在任何其他机器上中断，这有点偏离 Docker 的理念。因为这个原因，只能在 Dockerfile 中使用可移植的指令。


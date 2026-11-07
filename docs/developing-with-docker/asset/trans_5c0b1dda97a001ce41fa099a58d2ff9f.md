
![](img/0c87321919fa245e22f056c67ec70003_158_0.png)

VOLUME 与 -v 之间的根本区别在于：-v 会将操作系统中已有的文件挂载到 Docker 容器内部，而 VOLUME 会在主机上创建一个新的、空的卷并将其挂载到容器内部。

## WORKDIR

WORKDIR 指令为 Dockerfile 中后续的 CMD、RUN、ENTRYPOINT、COPY 或 ADD 指令添加一个工作目录。该指令的语法为 WORKDIR /PATH。你可以在一个 Dockerfile 中使用多个 WORKDIR 指令，如果提供了相对路径，则它将相对于上一个 WORKDIR 指令的路径。

## ARG

如果需要在 docker build 命令期间在构建时向 Docker 传递一个参数，你可以在 Dockerfile 中使用 ARG 指令。ARG 变量的定义从其在 Dockerfile 中定义的这一行开始生效。稍后，在构建过程中，可以使用 --build-arg 开关向 docker build 命令传递任何值：

```
--build-arg <variable name>=<value>
```

来自 --build-arg 开关的值将被传递给正在构建镜像的守护进程。如果指定了使用 ARG 指令未定义的构建时参数，构建将以错误失败。你可以使用多个 ARG 指令指定多个参数，例如：

```
FROM ubuntu
ARG user
ARG version
```

也可以在 Dockerfile 中为参数指定默认值——如果在构建时未指定，则将使用默认值：

```
FROM ubuntu
ARG user=jarek
```

可以通过调用以下命令将参数传递给构建器：

```
docker build --build-arg user=somebodyElse .
```

![](img/0c87321919fa245e22f056c67ec70003_159_0.png)

不建议使用 ARG 传递诸如 GitHub 密钥、用户凭据、密码等秘密信息，因为任何镜像使用者都可以通过 docker history 命令看到它。

## ONBUILD

ONBUILD 指令指定了额外的指令，当使用此镜像作为其基础镜像构建其他镜像时，这些指令将被触发。换句话说，ONBUILD 指令是父 Dockerfile 给子 Dockerfile（下游构建）的指令。任何构建指令都可以注册为触发器，并且这些指令将在 Dockerfile 中的 FROM 指令之后立即触发。

ONBUILD 指令的语法如下：

```
ONBUILD <INSTRUCTION>
```

其中，<INSTRUCTION> 是另一个 Dockerfile 构建指令，当子镜像即将构建时它将被触发。存在一些限制：ONBUILD 指令不允许链式使用另一个 ONBUILD 指令，并且不允许将 FROM 和 MAINTAINER 指令用作 ONBUILD 触发器。

如果你正在构建一个将用作构建其他镜像的基础的镜像，例如应用程序构建环境或守护进程，这些可能使用用户特定的配置进行自定义，那么这很有用。ONBUILD 指令对于自动化你选择的软件栈的构建非常有用。考虑以下使用 Maven 构建 Java 应用程序的示例。（是的，Maven 也可作为 Docker 容器使用）。基本上，你项目的 Dockerfile 需要做的就是引用包含 ONBUILD 指令的基础容器：

```
FROM maven:3.3-jdk-8-onbuild
CMD ["java","-jar","/usr/src/app/target/app-1.0-SNAPSHOT-jar-with-dependencies.jar"]
```

没有什么魔法，如果你查看父 Dockerfile，一切就会变得清晰；在我们的例子中，它将是 GitHub 上可用的 docker-maven Dockerfile：

```
FROM maven:3-jdk-8 RUN mkdir -p /usr/src/app WORKDIR /usr/src/app
ONBUILD ADD . /usr/src/app ONBUILD RUN mvn install
```

有一个基础镜像同时安装了 Java 和 Maven，并且有一系列用于复制文件和运行 Maven 的指令。

ONBUILD 指令向镜像添加一个触发指令，以便在稍后执行——当镜像用作另一个构建的基础时。触发器将在子构建的上下文中执行，就好像它已立即插入子 Dockerfile 中的 FROM 指令之后一样。

当 Docker 在构建过程中遇到 ONBUILD 指令时，构建器会向正在构建的镜像的元数据中添加一种触发器。但这就是该镜像被影响的唯一方式。在构建结束时，所有触发器的列表存储在镜像清单中，键为 OnBuild。你可以使用我们已经了解的 docker inspect 命令查看它们。

稍后，该镜像可能使用 FROM 指令用作新构建的基础。作为处理 FROM 指令的一部分，Docker 构建器会查找 ONBUILD 触发器，并按它们注册的相同顺序执行它们。如果任何触发器失败，FROM 指令将被中止，这将导致构建失败。如果所有触发器都成功，FROM 指令完成，构建继续。

![](img/0c87321919fa245e22f056c67ec70003_160_0.png)

ONBUILD 触发器在执行后会从最终镜像中移除。它们不会被进一步继承。

## STOPSIGNAL

要指定应发送到容器以使其退出的系统调用信号，请使用 STOPSIGNAL 指令。该信号可以是与内核 syscall 表中位置匹配的有效无符号数字，例如 9，或格式为 SIGNAME 的信号名称，例如 SIGKILL。

## HEALTHCHECK

这是一个相对较新的功能，于 Docker 1.12 中引入。你可以使用 HEALTHCHECK 指令告诉 Docker 如何测试容器以检查它是否仍在工作。这可以是检查数据库是否响应远程调用，或 Web 应用程序是否仍在指定的端口上监听。

如果你指定 HEALTHCHECK 指令，运行中的容器除了其正常状态外还将具有额外的状态。我们在第 3 章《理解容器和镜像》中讨论过容器状态。正如你所记得的，容器可以有多个状态，可以使用 docker ps 命令列出。这些状态可以是 created、restarting、removing、running、paused 或 exited。但有时这还不够——从 Docker 的角度来看，容器可能仍然“存活”，但应用程序可能会挂起或以其他方式失败——对应用程序状态进行额外检查会很有用，而 HEALTHCHECK 指令正派上用场。

HEALTHCHECK 状态最初为 starting。每当健康检查通过时，它就会变为 healthy（无论之前处于什么状态）。在连续失败一定次数后，它会变为 unhealthy。

HEALTHCHECK 指令的语法如下：

```
HEALTHCHECK --interval=<interval> --timeout=<timeout> CMD
<command>
```

<interval>（默认值是 30 秒）和 <timeout>（同样，默认值是 30 秒）是时间值，具体为检查间隔和超时时间。<command> 是实际用于检查应用程序是否仍在运行的命令。Docker 使用 <command> 的退出代码来确定健康检查是否失败或成功。值可以是 0，表示容器健康且可使用；也可以是 1，表示出现问题，容器未正常工作。

考虑以下示例：

```
HEALTHCHECK --interval=5m --timeout=2s --retries=3 CMD curl -f
http://localhost/ || exit 1
```

在前面的示例中，`curl -f http://localhost` 命令将每五分钟执行一次，最大超时时间为 2 秒。如果一次检查的运行时间超过 2 秒，则该检查被视为失败。如果连续三次重试失败，容器将获得 `unhealthy` 状态。

![](img/0c87321919fa245e22f056c67ec70003_162_0.png)

Dockerfile 中只能有一个 HEALTHCHECK 指令。如果列出多个，则只有最后一个 HEALTHCHECK 会生效。

HEALTHCHECK 指令使你能够精细调整容器监控，从而确保你的容器正常工作。这比仅靠 `running` `exited` 或 `dead` 这类标准 Docker 状态要好。

## SHELL

正如我们之前所见，一些指令如 RUN 和 CMD 可以使用 shell 形式的执行。Linux 上的默认 shell 是 ["/bin/sh", "-c"]，Windows 上是 ["cmd", "/S", "/C"]。如果需要，你可以使用 SHELL 指令覆盖 shell 形式命令使用的默认 shell。它可以是 Linux 上的 zsh、csh 或 tcsh，或 Windows 上的 PowerShell 等。该指令的语法是：

```
SHELL ["executable", "parameters"]
```

请注意，SHELL 只能接受 JSON 数组作为参数。SHELL 指令也可以用于修改 shell 的工作方式。例如，在 Windows 上使用 SHELL cmd /S /C /V:ON，可以修改默认 cmd shell 的行为（在这种情况下将是延迟环境变量扩展）。

SHELL 指令可以出现多次。每个 SHELL 指令都会覆盖之前的所有声明，但请注意，Dockerfile 中的每一行 SHELL 都会在生成的镜像中产生一个新的层。

现在我们已经介绍了所有 Dockerfile 指令以及在创建镜像中的作用。让我们现在关注镜像构建的过程。

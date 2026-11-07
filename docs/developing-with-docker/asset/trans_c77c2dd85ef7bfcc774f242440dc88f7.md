
容器可以通过三种方式识别：使用其名称或短 ID 或 UUID 标识符。标识符由 Docker daemon 生成。可能，最方便的选项是通过名称识别容器。记住一个友好的名称总比记住一个长字母数字标识符更容易。为容器定义名称会增加其含义，因此你一眼就能知道容器负责什么。假设名称为 `myWildfly` —— 你就会知道该容器是你的应用服务器。将 `abbb3434aa43` 作为容器标识符并不会让人清楚。要为 `docker run` 命令提供名称，请使用 `--name` 开关。如果你没有使用 `--name` 选项为容器提供名称，那么 Docker 会为你创建一个名称。我建议你始终为容器创建有意义的名称，因为这样会更容易记住它们。如果你指定了名称，你可以在 Docker 网络中引用容器时使用它。这适用于后台和前台 Docker 容器。

![](img/0c87321919fa245e22f056c67ec70003_173_0.png)

当容器在默认 bridge 网络上运行时，它们必须通过名称链接才能通信。

Docker 使用 PID 命名空间，从而为你提供监控容器内运行进程以及限制其数量的选项。

## PID settings

要设置 PID 命名空间模式，你有三种设置容器 PID 命名空间模式的选项：

- `--pid=""`: 这将为容器设置 PID（进程）命名空间模式
- `--pid=container`: 此 `<name | id>` 加入另一个容器的 PID 命名空间
- `--pid=host`: 这将在容器内部使用主机 PID 命名空间

默认情况下，所有容器都启用了 PID 命名空间。PID 命名空间对于进程分离很有用。默认情况下，容器 PID 与主机隔离，但你可以通过使用 `--pid host` 选项运行容器来更改此行为，容器 PID 将从主机可见。此外，你的容器将共享主机的进程命名空间。这意味着容器内的进程将看到主机上的所有进程。你可以使用 `--pid=container:id` 将要启动的容器加入另一个容器的 PID 命名空间。这可能有助于从另一个容器调试该容器；例如，你还可以轻松限制容器内运行的活动进程数量以防止 fork 炸弹。

fork 炸弹（也称为 rabbit 病毒或 wabbit）是一种拒绝服务攻击，其中一个进程不断复制自身以耗尽可用系统资源，导致系统因资源使用而变慢或崩溃。要限制容器内的活动进程数量，请使用设置了 --pids-limit 值的 docker run 命令执行：

```
docker run --pids-limit=64
```

使用 docker inspect 命令获取容器的当前 PID 很简单：

```
docker inspect --format '{{.State.Pid}}' <containerID or name>
```

另一方面，通过 PID 获取容器名称更棘手，但可行。为此，你需要使用一些 grep 操作，如以下示例所示：

```
docker ps -q | xargs docker inspect --format '{{.State.Pid}}, {{.ID}}' |
grep "^${PID},"
```

类似于某些程序将进程 ID 写入文件（你见过它们作为 PID 文件），Docker 可以将容器 ID 写入你选择的文件。要将进程 ID 写入文件，请使用 --cidfile 开关：

```
--cidfile=""
```

除了 PID 命名空间之外，你还可以为容器设置 UTS 命名空间。

## UTS settings

正如你记得的，Docker 试图将容器与主机系统隔离。UTS 命名空间用于设置主机名和域，即在该命名空间中运行进程时可见的。UTS 是 UNIX Timesharing System 的缩写。默认情况下，所有容器都启用了 UTS 命名空间。这意味着进程拥有主机名和（现在大多未使用）NIS 域名名的单独副本，因此可以将其设置为其他值而不影响系统的其余部分。

主机名通过 sethostname 设置，是 uname 返回的结构中的 nodename 成员。NIS 域名由 setdomainname 设置，是 uname 返回的结构中的 domainname 成员。

在 Docker 中设置 UTS 命名空间的语法与 --pid 语法非常相似，你只需使用 --uts 而不是 --pid。要为容器设置 UTS 命名空间，请使用 --uts="value" 开关。你也可以使用 --uts=host 在容器内绑定主机的 UTS 命名空间。

现在我们知道了如何识别容器，我们想要运行并设置它们的命名空间。让我们学习如何覆盖镜像 Dockerfile 中提供的默认命令。我们在前几章中已经做过，但没有深入细节。现在让我们再做一次。

## Overriding default commands from Dockerfile

正如你从第 6 章《创建镜像》中可以回忆起来的，当我们创建 Docker 镜像时，我们使用 CMD 指令或使用 ENTRYPOINT 指令提供运行时执行的默认命令或选项。但如果没有通过提供不同于镜像作者提供的命令或参数来影响容器启动过程的可能性，在许多情况下就无法使用 Docker。镜像会相当静态，有时甚至毫无用处。幸运的是，Docker 支持覆盖这些默认值，这一功能使其成为非常灵活的工具。正如你从上一章记得的，ENTRYPOINT 和 CMD 指令之间存在重要区别：ENTRYPOINT 定义了在容器中作为 PID 1 运行的进程，而 CMD 为其提供选项。如果在 Dockerfile 中未指定 ENTRYPOINT，但指定了 CMD，则 shell /bin/sh -c 将是镜像的入口点。CMD 指令与 ENTRYPOINT 相同，可以在执行 docker run 命令时在命令行中指定，这使其成为修改容器行为的强大工具。因此，在本节中我们关注覆盖启动命令及其参数；让我们从 CMD 开始。

## Overriding the CMD

正如你从本章开头记得的，docker run 命令的语法包括 COMMAND 参数：

```
docker run [OPTIONS] IMAGE[:TAG|@DIGEST] [COMMAND] [ARG...]
```

COMMAND 参数不是必需的，因为镜像作者可能已在镜像的 Dockerfile 中使用 CMD 指令提供了默认 COMMAND。这通常经常是这种情况。

CMD 指令应在 Dockerfile 中仅使用一次来运行镜像中包含的软件。它通常是 Dockerfile 中的最后一个命令，但当从此类镜像启动容器时，我们只需将自己的命令或参数作为 docker run 的 COMMAND 参数提供，即可覆盖 CMD 指令。docker run 命令中出现在镜像名称之后的所有内容都会传递给容器并被视为 CMD 参数。结果是，如果镜像还指定了 ENTRYPOINT，则 CMD 或 COMMAND 会作为参数附加到 ENTRYPOINT。但猜猜看，我们也可以覆盖 ENTRYPOINT。

## Overriding the ENTRYPOINT

每个 Docker 镜像都有一个入口点。它可以是默认的，也可以是由 ENTRYPOINT 指令指定的，它是运行时要执行的默认命令；它指定容器启动时运行什么可执行文件，但更难覆盖。ENTRYPOINT 使运行中的容器更像是一个要执行的完整命令，它从 COMMAND 值获取参数。但有时在运行容器时，你可能想运行其他东西，因此你可以在运行时使用字符串覆盖默认 ENTRYPOINT 来指定新的 ENTRYPOINT。覆盖入口点的语法如下：

```
docker run -it --entrypoint /bin/bash Ubuntu
```

让我们看看 MongoDB Dockerfile，它定义了 ENTRYPOINT：

```
FROM dockerfile/ubuntu
RUN mkdir -p /data/db /data/configdb \
&&chown -R mongodb:mongodb /data/db /data/configdb
VOLUME /data/db /data/configdb
COPY docker-entrypoint.sh /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]
EXPOSE 27017
CMD ["mongod"]
```

如你所见，定义了一个自定义入口点；它将是一个 entrypoint.sh shell 脚本。如果你现在运行此容器，容器的主进程将来自此脚本。我们可以使用以下命令覆盖此入口点：

```
docker run -it --entrypoint /bin/bash mongo
```

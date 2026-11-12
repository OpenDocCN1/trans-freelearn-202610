

作业可以定义一个 `tags` 列表，用于指定使用哪个运行器。在前面的示例中，镜像将由带有 `docker` 标签的运行器构建。如果设置了多个标签，运行器必须匹配所有标签才能被使用。这使得可以创建能够构建某些类型项目而不能构建其他项目的运行器。

最后，`script` 选项是用于构建项目的命令列表。每个命令必须在下一个命令运行之前成功。如果其中任何一个失败，构建将停止并标记为失败。命令可以是示例中的常规系统命令，也可以是来自仓库的脚本。

在此示例中，运行器先构建镜像。`--pull` 标志与 `docker build` 一起使用，以确保使用基础镜像的最新副本来构建新镜像。如果构建成功，新镜像将被推回仓库。可用镜像列表可在项目页面的 **镜像仓库** 部分中查看：

![](img/02bead9c8ff4515f78dac0eb33163961_233_0.png)

当前构建以及之前构建的状态可在项目的 **流水线** 部分中查看。点击任何构建可深入查看每个构建阶段的状态。点击阶段将显示该阶段的构建日志。失败构建生成的任何错误都将显示在那里：

![](img/02bead9c8ff4515f78dac0eb33163961_234_0.png)

## 测试镜像

CI 之所以在软件开发者中如此受欢迎，原因之一是它提供了一种方式，可以自动针对新构建运行测试套件，以确保其正常工作。测试提供了一种安全保障，确保无论开发者做出了哪些更改，都不会破坏软件的功能。这对于将要部署到潜在成百上千台服务器的 Docker 镜像同样有价值。

实现测试有多种方式：

- 第一种方法是将测试包含在 `git` 仓库中，但不将其添加到镜像中。这样可以保持镜像较小，但要求运行器上安装了测试所需的所有工具。某些运行器（例如 docker-in-docker 执行器）可能不包含运行测试所需的工具。

- 第二种方法是将测试包含在 Docker 镜像中。然后可以使用 `docker run` 或 `docker exec` 从容器内运行测试。这会导致镜像更大，但确保了测试镜像所需的所有工具都可用。它还使得可以随时在运行中的镜像上运行测试。如果某些问题仅在镜像在生产环境中运行时才会出现，这可能会很有用。

- 第三种选择是创建一个包含验证镜像所需所有测试的单独镜像。该镜像可以由 GitLab 在测试阶段下载并运行，以测试应用程序镜像。这种方法的好处是，随着新测试用例的出现，测试和测试镜像可以独立更新。

让我们看看本示例中使用的非常简单的网页：

```
<html>
<head><title>Docker Orchestration</title></head>
<body>
<h1>Docker Orchestration is fun!</h1>
<p>Automate all the things!</p>
</body>
</html>
```

老板决定网页必须始终宣称 Docker 编排有多么有趣。为此，创建了一个测试以确保它始终有趣。在此示例中，它位于 tests 目录中，并命名为 run-tests.sh：

```
#!/bin/sh
curl -s http://localhost | grep 'fun!</h1>' > /dev/null
```

此测试非常简单。它访问网页并检查 fun! 是否是标题中的最后一项。为了简化，测试将被添加到镜像中。更新后的 Dockerfile 如下。它做了两件事。首先，它添加了测试所需的 curl。其次，它将测试添加到镜像中的 /tests 目录：

```
FROM nginx:1.11
RUN apt-get update && apt-get install -y curl && apt-get clean
COPY index.html /usr/share/nginx/html/
COPY tests /tests
```

要运行测试，需要在 .gitlab-ci.yml 中添加一个新作业。以下代码片段显示了该作业。一旦 .gitlab-ci.yml 在仓库中更新，未来的提交将运行测试：

```
test-image:
  stage: test
  tags:
    - docker
  script:
    - docker pull $DOCKER_CI_IMAGE
    - docker run -d -P --name $CI_BUILD_ID $DOCKER_CI_IMAGE
    - docker run --rm --network container:$CI_BUILD_ID --name $CI_BUILD_ID-test
      --entrypoint /tests/run-tests.sh $DOCKER_CI_IMAGE
    - docker stop $CI_BUILD_ID; docker rm -f $CI_BUILD_ID
```

stage 选项被设置，以便作业将在 test 阶段运行。如果构建阶段失败，此作业不会运行。它还包含 Docker 标签，因此 GitLab 将使用也标记为 Docker 的运行器。

script 部分列出了用于测试镜像的命令。前两个命令下载镜像并在后台运行它。容器名称设置为 $CI_BUILD_ID，这是 GitLab 分配给构建的唯一构建 ID。设置名称很重要，因为运行测试时会用到它。

script 块中的第三个条目运行实际测试。它启动一个新容器，该容器共享目标容器的网络。这样测试就可以连接到 localhost 上的 Web 服务器。测试容器运行前面显示的测试脚本。如果脚本中的模式未匹配，grep 将失败，测试将返回非零退出码，GitLab 将其解释为失败。如果脚本中找到了该模式，测试成功，流程继续。

最后一行停止目标容器并将其删除。运行测试的容器在测试完成后会自动删除，因为启动它时使用了 --rm。

## 清理作业

前面的测试作业在测试成功时效果很好，但当测试失败时，它可能会让测试容器继续运行。当使用 GitLab.com 上的共享运行器时，这不是问题，因为每个测试都会启动一个新 VM，并在构建过程完成后立即将其删除。但是，当使用本地运行器时，这可能会成为问题。在这种情况下，可能需要一个作业在测试后进行清理。

首先，创建一个新的 `cleanup_test` 阶段，并定义一个名为 `cleanup_tests` 的作业在 `cleanup_test` 阶段中运行。通常，如果 `test` 阶段中的作业失败，之后的作业都不会运行。`cleanup_tests` 作业通过使用 `when` 选项来绕过这一限制。将其设置为 `always`，无论 `test` 阶段中发生什么，作业都会运行。`when` 选项还可以设置为 `on_success` 或 `on_failure`，以便仅在作业成功或失败时分别运行该作业。还有一个第四个选项 `manual`，将在本章后面介绍。以下代码片段展示了如何创建一个在 CI 过程完成后进行清理的作业：

```
stages:
  - build
  - test
  - cleanup_test
  - release
  - deploy

...

test-image:
  stage: test
  tags:
    - docker
  script:
    - docker pull $DOCKER_CI_IMAGE
    - docker run -d -P --name $CI_BUILD_ID $DOCKER_CI_IMAGE
    - sh tests/run-tests.sh $CI_BUILD_ID

cleanup_tests:
  stage: cleanup_test
  script:
    - docker stop $CI_BUILD_ID; docker rm -f $CI_BUILD_ID
  when: always
```

## 发布镜像

`build-image` 作业每次将更改推送到 `git` 仓库时都会创建一个更新的镜像。让我们更仔细地看看创建了什么。示例中的 `docker build` 命令正在创建名为 `registry.gitlab.com/perlstalker/ci-demo:$CI_BUILD_REF_NAME` 的镜像。`$CI_BUILD_REF_NAME` 变量是分支或标签的名称。这意味着，到目前为止，唯一创建的镜像是 `registry.gitlab.com/perlstalker/ci-demo:master`。没有 `:latest` 镜像。如果有人尝试运行 `docker run registry.gitlab.com/perlstalker/ci-demo`，该命令将失败，因为带有隐含 `latest` 标签的镜像不存在。


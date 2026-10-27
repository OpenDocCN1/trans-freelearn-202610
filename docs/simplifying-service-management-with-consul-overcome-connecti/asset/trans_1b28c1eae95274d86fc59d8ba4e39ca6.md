
好了，现在进入正题！在本节中，我们将创建三个 Consul 服务器，使用我们通过数据源找到的 AMI（即我们用 Packer 构建的 AMI）。我们使用 Terraform 脚本中创建的 SSH 密钥，并分配了一个公网 IP 地址，以便能够访问服务器。在脚本底部，我们为服务器关联了标签，以便轻松找到它们。帮你自己和你的 IT 运维团队一个忙：永远不要在未关联逻辑标签的情况下部署基础设施！其中一个标签还能唯一标识实例，我们可以用它来唯一识别服务器。在标签正上方，有一个 `user_data` 字段。这非常重要，因为它调用模板文件 `server_template.tpl` 来配置 Consul 服务器。我们还将 `server_name_tag` 属性传递给模板文件，以帮助识别服务器。代码如下所示：

```
resource aws_instance "consul-server" {
  count                         = 3
  ami                           = data.aws_ami.an_image.id
  instance_type                 = var.instance_type
  key_name                      = aws_key_pair.consul_key.key_name
  associate_public_ip_address   = true
  subnet_id                     = aws_subnet.consul-demo.id
  vpc_security_group_ids        = [aws_security_group.consul-demo.id]
  iam_instance_profile          = aws_iam_instance_profile.instance_profile.name
  user_data = templatefile("files/server_template.tpl", {
    server_name_tag = "${var.owner}-consul-server-instance"})
  tags = {
    Name     = "${var.owner}-consul-server-instance"
    Owner    = var.owner
    Instance = "${var.owner}-consul-server-instance-${count.index}"
  }
}
```

在下一个代码片段中，我们正在构建 Consul 客户端——准确地说，是四个，分别代表地、风、火、水四大核心元素。其中许多参数与服务器使用的参数相同，但请注意，这里没有 `user_data` 部分。为了多样化，我们使用 Terraform provisioner 来配置客户端，尽管在生产环境中，像 Ansible 这样的配置管理器会更受欢迎：

```
resource aws_instance "consul-client" {
  count = 4
  ami = data.aws_ami.an_image.id
  instance_type = var.instance_type
  key_name = aws_key_pair.consul_key.key_name
  associate_public_ip_address = true
  subnet_id = aws_subnet.consul-demo.id
  vpc_security_group_ids = [aws_security_group.consul-demo.id]
  iam_instance_profile = aws_iam_instance_profile.instance_profile.name
  tags = {
    Name = "${var.owner}-consul-client-instance-${count.index}"
    Owner = var.owner
  }
}
```

需要注意的是，远程 provisioner 在 Terraform 领域中并不是首选；不过，我偶尔发现它在定制所需资源时非常有用。作为此配置过程的一部分，我们遍历四个客户端中的每一个并执行一些功能，如下所示：

- 将名为 `httpd.json` 的服务定义文件传输到客户端。稍后当我们开始使用集群时，会进一步讨论这一点。
- 创建一个 `consul-client.hcl` 配置文件。我想更深入地探讨一下这个文件：
  - 我们宣告客户端的公网 IP 地址。这告诉 Consul 服务器以及任何请求访问此客户端上服务的设备，可以通过哪个地址访问它。我们选择客户端的公网地址，以便内部网络之外的应用程序可以访问它。
- 我们告诉 agent 不要以服务器模式运行，因为——嗯——我们是客户端。
- 我们启用脚本检查。这允许使用自定义脚本来检查服务健康状态，默认情况下是不启用的。
- 我们告诉 Consul 将其服务绑定到机器的私有地址。请注意绑定地址和宣告地址之间的区别。私有地址是机器自身知道的，并标识外部接口。而我们宣告的公网地址甚至不是客户端机器本身所知道的，它由 AWS 管理。
- 我们告诉客户端服务器在哪里。请记住，客户端需要向服务器注册，以便传达自身及其所有受管服务的状态。
- 我们指定与 Consul 服务器通信时使用的客户端地址。由于服务器和客户端位于同一个 AWS 虚拟网络中，我们可以使用私有 IP 地址。
- 将客户端配置和服务定义文件移动到配置目录（`/etc/consul/consul.d/`）。
- 使用 Python 启动一个简单的超文本传输协议（HTTP）服务器。毕竟，创建一个 Consul 集群却不在上面运行任何服务，何必这么麻烦呢？！
- 接下来，我们使用一个 provisioner 在客户端机器上远程启动 Consul 服务。
- 最后一块告诉 Terraform 它将如何连接到客户端机器。请注意，我们使用的是之前创建的 SSH 密钥的内容。

请注意，虽然客户端的配置全部都在这个 Terraform 文件中，但服务器的应用程序配置却不在这里。这是因为我们在 Terraform 中使用了 `user_data` 功能，而且我们很快就会看到模板文件。在 Terraform 中，当我们使用 provisioner 来执行脚本或移动文件时，我们会声明一个 `null_resource` 属性，如下所示：

```
resource null_resource "provisioning-clients" {
  for_each = { for client in aws_instance.consul-client : client.tags.Name => client }
  provisioner "file" {
    source      = "files/httpd.json"
    destination = "/tmp/httpd.json"
  }
}
```

```
```hcl
provisioner "remote-exec" {
  inline = [
    "sudo cat << EOF > /tmp/consul-client.hcl",
    "advertise_addr = \"${each.value.public_ip}\"",
    "server = false",
    "enable_script_checks = true",
    "bind_addr = \"${each.value.private_ip}\"",
    "retry_join = [\"${aws_instance.consul-server[0].public_ip}\",\"${aws_instance.consul-server[1].public_ip}\",\"${aws_instance.consul-server[2].public_ip}\"]",
    "client_addr = \"${each.value.private_ip}\"",
    "EOF",
    "sudo mv /tmp/consul-client.hcl /etc/consul/consul.d/consul-client.hcl",
    "sudo mv /tmp/httpd.json /etc/consul/consul.d/httpd.json",
    "nohup python3 -m http.server 8080 &",
    "sleep 60"
  ]
}
provisioner "remote-exec" {
  inline = [
    "sudo systemctl start consul",
  ]
}
connection {
  type        = "ssh"
  user        = "ubuntu"
  private_key = local_file.private_key.content
  host        = each.value.public_ip
}
```
```

接下来，我们有实例身份信息和相关角色。这与 Consul 没有直接关联，但它是启用我们在 AWS 内所需通信路径所必需的。代码如下所示：

```
```hcl
resource "aws_iam_instance_profile" "instance_profile" {
  name_prefix = var.owner
  role        = aws_iam_role.instance_role.name
}

resource "aws_iam_role" "instance_role" {
  name_prefix        = var.owner
  assume_role_policy = data.aws_iam_policy_document.instance_role.json
}

data "aws_iam_policy_document" "instance_role" {
  statement {
    effect  = "Allow"
    actions = ["sts:AssumeRole"]

    principals {
      type        = "Service"
      identifiers = ["ec2.amazonaws.com"]
    }
  }
}

resource "aws_iam_role_policy" "metadata_access" {
  name   = "metadata_access"
  role   = aws_iam_role.instance_role.id
  policy = data.aws_iam_policy_document.metadata_access.json
}

data "aws_iam_policy_document" "metadata_access" {
  statement {
    effect = "Allow"
    actions = [
      "ec2:DescribeInstances",
    ]
    resources = ["*"]
  }
}
```
```

好了——现在我们已经看完了整个 Terraform 文件，让我们仔细看看那个服务器模板文件。通过该模板，我们创建了一个 `consul-server.hcl` 服务器配置文件，并用所有必要的配置填充它，如下所示：

```
```bash
#!/usr/bin/env bash

cat << EOF > /etc/consul/consul.d/consul-server.hcl
server = true
bootstrap_expect = 3
retry_join = ["provider=aws tag_key=Name tag_value=${server_name_tag}"]
bind_addr = "$(curl -s http://169.254.169.254/latest/meta-data/local-ipv4)"
advertise_addr = "$(curl -s http://169.254.169.254/latest/meta-data/public-ipv4)"
client_addr = "$(curl -s http://169.254.169.254/latest/meta-data/local-ipv4)"
ui = true
EOF

# Starting consul services
sudo systemctl start consul
```
```

顺着这个模板文件，让我们看看这个文件到底在做什么：

- 1. 指示 Consul agent 以服务器模式运行。
- 2. 接下来，我们指定希望在集群中看到的预期服务器数量。请记住，我们之前在讨论共识和法定人数时提到过这一点。我们创建了三个 Consul 服务器，这意味着我们的法定人数是……（我给你一个提示——它大于一，小于三）。
- 3. 好的——这部分相当酷。如果你还记得，在 Terraform 脚本中，我们有一个 `instance` 标签，用于标识各个服务器。这个 `retry_join` 配置标识了一个服务器列表，这些服务器组成了预期的三个服务器对等集。AWS 会用与该 `instance` 标签匹配的服务器来填充这个值。


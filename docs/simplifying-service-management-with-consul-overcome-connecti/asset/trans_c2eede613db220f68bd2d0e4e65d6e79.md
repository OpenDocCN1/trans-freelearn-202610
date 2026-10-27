

```
```hcl
}
provisioner "file" {
  source      = "files/dc1-server-consul-${count.index}-key.pem"
  destination = "/tmp/dc1-server-consul-${count.index}-key.pem"
}
provisioner "file" {
  source      = "files/server_acl.hcl"
  destination = "/tmp/server_acl.hcl"
}
provisioner "remote-exec" {
  inline = [
    "sudo mv /tmp/*.pem /etc/consul/consul.d/"
  ]
}
connection {
  type        = "ssh"
  user        = "ubuntu"
  private_key = local_file.private_key.content
  host        = self.public_ip
}
}
```
```

需要关注的主要点是 `user_data` 脚本中的细微变化。与其简单地通过配置传递服务器名称，我们还包含了索引。这是因为我们用于证书的文件都遵循相同的命名结构，并且索引嵌入在文件名中。这些文件都可见于 `files` 文件夹（`$github_root/ch3/files`）：

- `files/consul-agent-ca-key.pem`
- `files/consul-agent-ca.pem`
- `files/dc1-server-consul-0-key.pem`
- `files/dc1-server-consul-0.pem`
- `files/dc1-server-consul-1-key.pem`
- `files/dc1-server-consul-1.pem`
- `files/dc1-server-consul-2-key.pem`
- `files/dc1-server-consul-2.pem`

`consul-agent-ca-key.pem` 文件是 CA 的私钥（Consul 是我们的 CA）。公共 CA 文件 `consul-agent-ca.pem` 已作为镜像构建过程的一部分推送到所有机器。通过在 `aws_instance` 资源中使用 provisioner，我们将剩余文件传输到三个服务器（0、1 和 2）中的每一个，并将它们放置在 `/etc/consul/consul.d/` 目录中。

利用文件 provisioner，我们还在服务器上添加了一个 ACL 配置文件。这只是为了帮助我们在开始使用 ACL 时使用。当我们配置客户端时，我们必须做一些小的更改。首先，我们必须将一个新的配置文件移动到客户端以帮助 ACL 配置。我们还将 CA 文件添加到配置中：

```
```hcl
resource null_resource "provisioning-clients" {
  for_each = { for client in aws_instance.consul-client :
  client.tags.Name => client }
  provisioner "file" {
    source      = "files/httpd.json"
    destination = "/tmp/httpd.json"
  }
  provisioner "file" {
    source      = "files/client_acl.hcl"
    destination = "/tmp/client_acl.hcl"
  }
  provisioner "remote-exec" {
    inline = [
      "sudo cat << EOF > /tmp/consul-client.hcl",
      "advertise_addr = \"${each.value.public_ip}\"",
      "server = false",
      "enable_script_checks = true",
      "bind_addr = \"${each.value.private_ip}\"",
      "retry_join = [\"${aws_instance.consul-server[0].public_ip}\",\"${aws_instance.consul-server[1].public_ip}\",\"${aws_instance.consul-server[2].public_ip}\"]",
      "client_addr = \"${each.value.private_ip}\"",
      "ca_file = \"/etc/consul/consul.d/consul-agent-ca.pem\"",
      "verify_incoming = true",
      "verify_outgoing = true",
      "verify_server_hostname = true",
      "auto_encrypt = {"
```
```

```
```hcl
  " tls = true",
  "}",
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
  // private_key = file("~/hashicorp/aws/rjackson-east2.pem")
  host        = each.value.public_ip
}
}
```

```

除了将 CA 文件添加到配置中之外，我们还添加了一些其他参数：

```
```hcl
"verify_incoming = true",
"verify_outgoing = true",
"verify_server_hostname = true",
"auto_encrypt = {",
"  tls = true",
"}",
```
```

```
```
}
connect {
  enabled = true
}
EOF
sudo systemctl start consul
```
```

与客户端类似，我们将验证参数设置为确保使用证书文件验证接收的消息以及发送的消息。那些文件在接下来的几行中配置，但请注意，与客户端不同，我们为每个服务器配置了私钥和公钥。服务器集群必须以安全的方式配置和创建，然后才能使用 auto_encrypt 配置启用向客户端分发证书。我想指出配置的端口。在此配置中，我们将端口 8500 配置为 HTTP，这就是我们在 *第 2 章，架构 – 它是如何工作的？* 中使用的方法。然而，我们还具有 HTTPS 配置，端口 8501 专门用于与 Consul 集群的 TLS 通信。因此，如果你禁用 HTTP（将 `http = -1` 设置），你将通过阻止任何未加密的 HTTP 流量来保护你的 Consul 系统。如果你想看看未认证的网页浏览器尝试与 Consul 通信时会是什么样子，假设 Consul 系统正在运行，将你的网页浏览器指向 Terraform 输出中提供的 `CONSUL_HTTPS_ADDR` 参数之一。你会看到类似以下内容：

![](img/52479dafaefba00517c48a137e037b7e_105_0.png)

图 3.8 – 未授权的 HTTP 访问

## 保持安全、愚蠢，并保护你的集群！

如果你检查证书，你会看到它是由 Consul CA 签发的并且不受信任。这是因为证书没有由官方 CA 签名；它仅由 Consul 本身签名。

![](img/52479dafaefba00517c48a137e037b7e_106_0.png)

图 3.9 – 用于 HTTP 通信的证书

值得庆幸的是，我们仍然打开了端口 8500，以便无阻碍地与服务器集群通信。

但等等，还有更多！你还记得我们说过保护通信主要有两条路径吗？一条是通过相互 TLS，使用证书进行 RPC 通信。那我们所有服务器的 gossip 呢？没错，我们也必须保护它，但不用担心，这要容易得多，而且像证书一样，我已经提前为你设置好了。

如果你查看 `consul-common.hcl` 文件，你会看到一个额外的行，其中包含一个简单的参数 `encrypt`。该文件已作为使用 Packer 的镜像构建过程的一部分放置在所有服务器上：

```
log_file = "/etc/consul/logs/"
log_level = "DEBUG"
encrypt = "86753098Qr2LsXLcfD+Y+S6RUuzUZsY1ZxdAj5E9c="
encrypt_verify_incoming = true
encrypt_verify_outgoing = true
```

这个值并不是我凭空编造的——好吧，不完全是。通过使用 `consul keygen` 命令，你可以生成一个加密密钥，然后用于加密节点之间的 gossip。由于该参数位于 `common` 配置中，所有节点都使用相同的密钥。我们还确保每个代理都在加密其流量（`encrypt_verify_outgoing`）并且只接受已加密的流量（`encrypt_verify_incoming`）。如果你想更新此加密密钥（强烈建议），你可以使用 `consul keyring` 命令。

此时，你已经拥有一个完全设置好的 Consul 集群，所有节点之间运行受保护的消息，既用于 RPC 通信，也用于 gossip 通信。如我之前所述，将证书和加密参数作为构建的一部分包含在内的决定是为了简化流程。根据我的经验，创建和分发证书的过程具有挑战性，如果你没有机器的官方注册域名则更具挑战性。此过程应由你的安全团队完成，因此在设计 Consul 机器之间的安全通信时，请务必让他们参与其中。

## 让我们玩玩一些 ACL！

现在我们的集群已经启动并运行，让我们开始玩一些 ACL。我不想在构建过程中启用 ACL，仅仅因为没有令牌，我们将无法访问我们的 Consul 系统！我还想让你看到系统在默认拒绝策略下是如何被锁定的，然后我们再慢慢打开它。我们在这里将使用 SSH 和构建过程中创建的密钥在机器之间频繁跳转。在真实场景中，像我们的构建一样，其中很大一部分，甚至全部，都可以用 Terraform 作为代码执行，所以如果你觉得这个过程很痛苦，你可能想接下来读一本 Terraform 书籍。




"/packer/scripts/install-updates.sh",
"/packer/scripts/install-docker.sh"
  ]
}
]
}

首先是 builders 部分。唯一的条目是 qemu 构建器，由 type 设置指定。iso_* 设置指定要使用的镜像 URL、用于验证镜像的校验和文件以及校验和类型。在此例中，只有一个单独的镜像文件。由于镜像是磁盘镜像而非 ISO，因此 disk_image 设置必须设置为 true。

format 设置将输出格式设置为 qcow2。format 也可以设置为 raw。disk_size 设置以 MB 为单位设置新磁盘镜像的大小。这设置了磁盘的大小但不会更改文件系统。当使用磁盘镜像启动 VM 时，Cloud-Init 会调整根文件系统的大小。输出目录和生成的镜像名称分别在 output_directory 和 vm_name 选项中设置。output_directory 中设置的路径可以是绝对路径，也可以是相对于运行 Packer 的位置的相对路径。

默认情况下，qemu 构建器将使用 SDL 控制台启动 VM。只有在可用 X11 时才有效。在大多数情况下，headless 设置应设置为 true。在预配置过程中，控制台可通过 VNC 访问以查看控制台。

ssh_* 设置控制 qemu 构建器如何为预配置连接到 VM。ssh_username 和 ssh_password 设置设置登录 VM 所需的凭据。在此例中，它配置为使用 root 用户和准备镜像时设置的密码。VM 启动后，qemu 构建器将不断尝试连接到 SSH，但在 ssh_wait_timeout 指定的时间后会放弃。确保超时时间为 VM 启动提供足够的时间。

provisioners 部分是用于预配置镜像的预配置器列表。此示例中有一个 shell 预配置器，它在 VM 上运行 scripts 部分中指定的脚本。install-updates.sh 脚本只是运行 apt-get update && apt-get dist-upgrade -y。第二个脚本 install-docker.sh 是来自 get.docker.com 的安装脚本。

配置就绪后，可以通过运行 `packer build <config>` 来构建镜像，其中 `<config>` 是配置文件的名称。此处给出了一个示例。为简洁起见，大部分输出已被删除：

```
$ packer build xenial-cloudimg.json
qemu output will be in this color.
```

```
==> qemu: Downloading or copying ISO
qemu: Downloading or copying: file:///packer/images/xenial-server-cloudimg-amd64-prep.img
==> qemu: Copying hard drive...
==> qemu: Resizing hard drive...
==> qemu: Found port for communicator (SSH, WinRM, etc): 3475.
==> qemu: Looking for available port between 5900 and 6000 on 127.0.0.1
==> qemu: Starting VM, booting disk image
qemu: The VM will be run headless, without a GUI. If
you want to
qemu: view the screen of the VM, connect via VNC without a password to
qemu: 127.0.0.1:5983
==> qemu: Waiting 10s for boot...
==> qemu: Connecting to VM via VNC
==> qemu: Typing the boot command over VNC...
==> qemu: Waiting for SSH to become available...
==> qemu: Connected to SSH!
==> qemu: Provisioning with shell script: /packer/scripts/install-updates.sh
==> qemu: Provisioning with shell script: /packer/scripts/install-docker.sh
==> qemu: Gracefully halting virtual machine...
==> qemu: Converting hard drive...
Build 'qemu' finished.

==> Builds finished. The artifacts of successful builds are:
--> qemu: VM files in directory: /packer/images/xenial-cloudimg
```

![](img/02bead9c8ff4515f78dac0eb33163961_257_0.png)

使用 `qemu` 构建器时，请确保运行 Packer 的用户有权使用 KVM。如果用户没有所需的访问权限，Qemu 可能会抛出权限拒绝错误。在 Ubuntu 上，用户需要加入 `kvm` 组。

运行需要几分钟，假设一切正常，新镜像将放置在 `output_directory` 设置的目录中。除非设置了 `-force` 选项以避免意外删除构建镜像，否则如果输出目录已存在，Packer 不会运行。

## 将 Puppet 与 Packer 结合使用

如果需要的更改相对简单，使用脚本预配置新镜像有效。更复杂的更改需要更复杂的脚本。配置管理工具非常适合此任务，可用于预配置新镜像。Ansible、Chef、Puppet 和 Salt 均受支持。

使用配置管理工具预配置镜像的额外好处是，可以使用相同的配置来确保服务器运行时保持正确配置。具体做法会因所使用的工具而异。

以下 Packer 代码片段使用 Puppet 的无主模式来配置镜像。与“使用配置管理”部分中使用的相同 Puppet 清单在此处也被使用。不应包含加入 Docker 集群的命令。在集群中启动新实例后，可以运行加入集群的命令：

```
"provisioners" : [
{
	"type" : "shell",
	"scripts" : [
		"/packer/scripts/install-updates.sh",
		"/packer/scripts/install-puppet.sh"
	]
},
{
	"type" : "puppet-masterless",
	"manifest_file" : "/packer/puppet/docker-host.pp"
}
]
```

第一个区别是 install-docker.sh 脚本被 install-puppet.sh 取代，后者运行 apt-get install -y puppet。第二个是带有 puppet-masterless 类型的新预配置器块。manifest_file 选项指定将用于配置镜像的清单；也可以使用目录。在这种情况下，所有清单都将应用于节点。

## 将基础架构视为代码

让我们停下来考虑一下刚才所做的事情。服务器已完全在一个或多个文件中定义，并具有可脚本化的构建过程。我们刚刚将服务器变成了代码。

服务器镜像可以通过 CI 过程构建。与在运行中的服务器上执行更新不同，可以构建包含更新的新镜像。镜像在部署前可以通过测试套件进行全面测试。在部署到生产环境之前，可以在测试环境中对它们进行全面审查。甚至部署过程也可以通过 CI 过程来控制，就像 Docker 镜像一样。

这种方法的另一个好处是回滚更新非常容易。如果发现问题，可以在先前可用的镜像上启动新服务器以快速恢复服务。有问题的镜像可以离线或在不同环境中进行测试和调试。

基本上，这是运行版本化容器的相同方法。从故障排除角度来看，重要的是日志等重要数据不存储在服务器上。相反，它们应存储在网络存储上或发送到 Elasticsearch 和 Kibana 等服务。

## 构建后配置

使用自定义镜像并不能消除对启动时和持续配置管理的需求。启动时配置可以最小化为主机名或加入集群等简单事项。在多区域或多云环境中运行时，Cloud-Init 会设置特定于区域的信息。

对运行中的系统进行配置管理仍然很重要。它可以确保构建服务器时所做的配置保持不变。它还可以进行简单的更改，例如更新管理员 SSH 密钥，而无需完全重建镜像。

## 自动化主机部署

自动化 Docker 主机部署有很多种方式。具体细节，如往常一样，取决于编排系统。例如，向在 Amazon EC2 上运行的 Docker Swarm 集群添加新主机，就像运行类似下面的内容一样简单：

```
$ docker-machine create --driver amazonec2
$ docker swarm join --token ABC... manager
```

对于 Azure 或 GCE，也是同样的操作。只需更改驱动程序，并将相应的驱动程序特定标志传递给 docker-machine 即可。

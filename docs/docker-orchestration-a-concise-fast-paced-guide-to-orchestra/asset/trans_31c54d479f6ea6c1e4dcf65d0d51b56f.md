

## 安装 Weave

第一步是将 `weave` 工具安装到你的桌面或其他管理系统中。`weave` 工具只是一个 shell 脚本。weave 的核心功能在 Docker 容器中运行。该脚本也应直接安装在 Docker 节点本身：

```
$ wget -O weave git.io/weave
$ chmod +x weave; cp weave /usr/local/bin/
```

以下示例假设一个三节点集群，节点名称为 `swarm-master`、`swarm-host1` 和 `swarm-host2`。节点可以使用 Docker Machine 创建，如 *第 1 章，开始使用 Docker 编排* 中所述。可以向集群添加任意数量的主机。网格越大，在发生节点故障时的弹性就越强。

下一步是初始化 Weave。首先使用 Docker Machine 切换到第一台服务器。`consensus` 标志告诉 weave 最初将有三台服务器。以后可以添加更多：

```
$ eval $(docker-machine env swarm-master)
$ weave launch --ipalloc-init consensus=3
```

这将在 `swarm-master` 上下载并启动三个容器。如果要加密网格，请将 `--password` 标志添加到 `weave launch`。密码在连接的每个节点上必须相同：

```
$ weave launch --password XXXX ...
```

你可以使用 `docker ps` 验证它们是否正在运行：

```
$ docker ps
CONTAINER ID        IMAGE                               COMMAND                   NAMES
CREATED             STATUS    PORTS
b645e00cde8d        weaveworks/plugin:1.6.1         "/home/weave/plugin"      weaveplugin
42 seconds ago      Up 41 seconds

e0bc711a0972        weaveworks/weaveexec:1.6.1        "/home/weave/weavepro"    weaveproxy
44 seconds ago      Up 43 seconds

89897514f42d        weaveworks/weave:1.6.1            "/home/weave/weave --"    weave
45 seconds ago      Up 44 seconds
```

接下来，在第二台主机上安装 weave 并将其连接到第一台：

```
$ eval $(docker-machine env swarm-host1)
$ weave launch --ipalloc-init consensus=3
$ weave connect 10.128.0.2
```

最后一条命令中的 IP 地址是主节点的 IP 地址。通常可以在那里使用 `$(docker-machine ip swarm-master)`。如果你在 GCE 等服务上运行主机，则希望使用那里的内部 IP 地址，因为它不会改变。
对第三台主机重复此过程：

```
$ eval $(docker-machine env swarm-host2)
$ weave launch --ipalloc-init consensus=3
$ weave connect 10.128.0.2
```

一旦 weave 在所有三个节点上安装并运行，使用 weave status 检查网络状态：

```
$ weave status
Version: 1.6.1 (up to date; next check at 2016/09/16 23:30:55)
Service: router
Protocol: weave 1..2
Name: 22:b6:71:6d:9c:d2 (swarm-host2)
Encryption: disabled
PeerDiscovery: enabled
Targets: 1
Connections: 2 (2 established)
Peers: 3 (with 6 established connections)
TrustedSubnets: none
Service: ipam
Status: idle
Range: 10.32.0.0/12
DefaultSubnet: 10.32.0.0/12
Service: dns
Domain: weave.local.
Upstream: 169.254.169.254
TTL: 1
Entries: 0
Service: proxy
Address: tcp://104.197.144.79:12375
Service: plugin
DriverName: weave
```

## 将容器连接到 Weave

既然网络已经设置好，是时候启动使用它的容器了。容器将使用启动 weave 时创建的 weave 网络。本示例将启动名为 shell11 和 shell12 的两个容器。shell11 容器将在后台运行：

```
$ docker run --net=weave -h shell1.weave.local $(weave dns-args) -tdi weaveworks/ubuntu
```

```
$ docker run --net=weave -h shell2.weave.local $(weave dns-args) -ti \
weaveworks/ubuntu
```

为了让 DNS-SD 工作，需要在 `docker` `run` 中使用 `--dns` 和 `--dns-search` 选项。weave 提供一个名为 `weave dns-args` 的帮助命令，它将适当地设置这些选项。

执行第二条命令后，你将留在 `shell12` 容器的命令提示符处。从那里，你应该能够 ping `shell11`：

```
root@shell12:/# ping shell11
PING shell11 (10.40.0.0) 56(84) bytes of data.
64 bytes from 89d43b8ac937.weave (10.40.0.0): icmp_seq=1 ttl=64
time=0.180 ms

64 bytes from 89d43b8ac937.weave (10.40.0.0): icmp_seq=2 ttl=64
time=0.099 ms

64 bytes from 89d43b8ac937.weave (10.40.0.0): icmp_seq=3 ttl=64
time=0.069 ms

64 bytes from 89d43b8ac937.weave (10.40.0.0): icmp_seq=4 ttl=64
time=0.104 ms
^C
--- shell11 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3000ms
rtt min/avg/max/mdev = 0.069/0.113/0.180/0.040 ms
```

## 使用 Flannel

Flannel 是由 CoreOS 开发的覆盖网络。它使用 `etcd` 作为数据存储并进行网络同步。Flannel 缺少 Docker overlay 和 weave 支持的一些功能。没有加密或 DNS-SD 的选项。但是，一旦配置完成，它使用起来非常简单。

## 配置 etcd 和 Flannel

CoreOS 通过 `cloud-config` 配置。`cloud-config` 文件是一个 YAML 文件，通过用户数据选项传递给主机。文件必须以 `#cloud-config` 开头，CoreOS 才能识别它。

在开始之前，必须为 `etcd` 生成一个新令牌。`size` 参数是集群的初始大小。以后可以添加和删除节点：

```
$ wget -qO - 'https://discovery.etcd.io/new?size=3'
https://discovery.etcd.io/c37921aac12231bf487a3ed82c464a4f
```

发现 URL 可以接受其他选项。有关详细信息，请参阅 https://coreos.com/etcd/docs/latest/clustering.html。

生成的令牌将在 cloud-config 文件的 coreos > etcd2 > discovery 选项中使用。你需要为集群中的每个主机使用相同的令牌。每个集群都必须生成一个新令牌：

```
#cloud-config

coreos:
etcd2:
discovery: https://discovery.etcd.io/<token>
advertise-client-urls:
http://$private_ipv4:2379,http://$private_ipv4:4001
initial-advertise-peer-urls: http://$private_ipv4:2380
listen-client-urls:
http://0.0.0.0:2379,http://0.0.0.0:4001
listen-peer-urls: http://$private_ipv4:2380
flannel:
public_ip: $private_ipv4
etcd_endpoints: http://$private_ipv4:2379
fleet:
public_ip: $private_ipv4
units:
- name: etcd2.service
command: start
- name: fleet.service
command: start
- name: flanneld.service
command: start
drop-ins:
- name: 50-networking-config.conf
content: |
[Service]
ExecStartPre=/usr/bin/etcdctl set
/coreos.com/network
config '{"Network":"10.42.0.0/16"}'
- name: docker.service
command: start
drop-ins:
- name: 40-flannel.conf
content: |
[Unit]
Requires=flanneld.service
After=flanneld.service
```

此示例展示了如何在 CoreOS 上使用 Flannel。它假设你的 CoreOS 主机有两个网络接口；一个连接到公共网络，一个连接到专用网络。$private_ipv4 变量将被云提供商分配的专用 IP 地址替换。这受到 Amazon EC2、GCE、Microsoft Azure 和 OpenStack 的支持。

Flannel 将其配置以 JSON 格式存储在 etcd 的 /coreos.com/network/config 路径中。在 Flannel 启动之前，必须将配置写入 etcd。由于 CoreOS 使用 systemd 来管理服务，因此可以创建一个 drop-in 文件，在 Flannel 启动之前将配置添加到 etcd。

前面的示例具有 Flannel 所需的最低限度配置。它所做的只是设置将用于为每个容器分配 IP 地址的网络。你应该将其更改为与你的环境中任何内容都不冲突的内容。如果 Docker 已经在运行，则需要重新启动 Docker。Flannel 文档中提供了完整的选项列表，网址为 https://github.com/coreos/flannel。

## 使用 Flannel 启动 CoreOS 集群

以下命令在 GCE 上启动三台 CoreOS alpha 服务器。--can-ip-forward 标志是 Flannel 正确转换运行中容器的流量所必需的。主机的 cloud-config 文件通过 --metadata-from-file 选项设置：

```
$ gcloud compute instances create core1 core2 core3 --image-project coreos-cloud --image-family coreos-alpha \
--zone us-central1-a --machine-type g1-small --can-ip-forward --scopes compute-rw --metadata-from-file \
user-data=coreos-user-data.yml
```

确保 sshd 已启动：

```
$ gcloud compute instances get-serial-port-output core1 | grep
Starting.*sshd
Starting Generate sshd host keys...
```

一旦主机启动，登录到其中一台以验证 etcd 是否健康。由于 Flannel 依赖于 etcd，如果它不起作用，Flannel 也不会起作用：

```
$ gcloud compute ssh --zone us-central1-a core@core1
X11 forwarding request failed
Last login: Sat Sep 17 21:34:41 2016 from 162.220.127.20 core@core1 ~ $
etcdctl cluster-health
member 9817b5cb6db4b548 is healthy: got healthy result from
http://10.128.0.4:2379
member aa7418b2c3e34652 is healthy: got healthy result from
```


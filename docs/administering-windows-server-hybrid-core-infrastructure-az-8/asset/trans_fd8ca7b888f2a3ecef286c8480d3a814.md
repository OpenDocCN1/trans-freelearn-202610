

3. Hyper-V 支持哪些虚拟处理器调度类型？
- A. 经典调度程序
- B. 核心调度程序
- C. 根调度程序

4. Hyper-V 支持哪些虚拟网络交换机类型？
- A. 公共
- B. 私有
- C. 内部
- D. 外部

5. 增强会话模式可访问以下哪些本地资源？
- A. 驱动器
- B. USB 设备
- C. 智能卡
- D. 打印机
- E. 以上全部

6. 管理 Hyper-V Windows Server VM 的远程访问方法有哪些？
- A. PowerShell Remoting
- B. PowerShell Direct
- C. RDP
- D. HVC.exe

7. Windows Server 容器中 Hyper-V 隔离模式的哪些特征是正确的？
- A. 每个容器都有用户模式
- B. 容器与主机之间存在共享内核
- C. 每个容器与主机之间提供硬件级隔离
- D. 每个容器在 VM 内运行

8. 以下哪些是 Windows Server 容器的网络驱动程序？
- A. L2bridge 网络驱动程序
- B. L2tunnel 网络驱动程序
- C. NAT 网络驱动程序
- D. Overlay 网络驱动程序
- E. 以上全部

9. 哪个 Windows Server 容器基础映像最适合 lift and shift 场景？
- A. Nano Server
- B. Server Core
- C. Windows
- D. Windows Server

10. 哪个命令(或命令)通过 DDA 使设备可用于 VM？
- A. Mount-VMHostAssignableDevice -LocationPath $locationPath
- B. Add-VMAssignableDevice -LocationPath $locationPath -VMName VMName
- C. Dismount-VMHostAssignableDevice -LocationPath $locationPath
  然后，使用以下命令：

  ```
  Add-VMAssignableDevice -LocationPath $locationPath
  -VMName VMName
  ```
- D. Remove-VMAssignableDevice -LocationPath $locationPath -VMName VMName
  然后，使用以下命令：

  ```
  Set-VMAssignableDevice -LocationPath $locationPath
  -VMName VMName
  ```

## Practice test 4 – 实现和管理本地和混合网络基础架构

1. 哪种混合网络连接解决方案要求您的 IP 地址空间不与本地重叠？
- A. VNet 对等互连
- B. Azure VPN 网关
- C. Azure NAT 网关
- D. Azure 扩展网络

2. 哪些混合远程访问解决方案提供限时访问？
- A. 恰好足够访问 (JEA)
- B. 即时访问 (JIT)
- C. Azure 堡垒服务
- D. 远程桌面服务 (RDS)

3. 以下哪些功能不是 Azure DNS 提供的？
- A. 反向 DNS 查找
- B. 有条件转发
- C. DNSSEC
- D. 区域传输
- E. 以上全部

4. DNS 区域必须具有信任锚才能被识别为支持 DNSSEC。
- A. 正确
- B. 错误

5. 当服务器收到为期 20 天的 DHCP 租约时，将在哪一天尝试续约？
- A. 第 20 天
- B. 第 19 天
- C. 第 10 天
- D. 第 15 天

6. 可以使用哪些 PowerShell cmdlet 来管理 DHCP 作用域？
- A. Add-DhcpServerv4Scope
- B. Get-DhcpServerv4Scope
- C. New-DhcpServerv4Scope
- D. Get-DhcpServerv4ScopeStatistics

7. 以下哪种是最不安全的身份验证协议？
- A. 密码身份验证协议 (PAP)
- B. 质询握手身份验证协议 (CHAP)
- C. Microsoft 质询握手身份验证协议版本 2 (MS-CHAPv2)
- D. 可扩展身份验证协议 (EAP)

8. Azure AVD 会话主机需要作为 Azure VM 的 AD 副本域控制器。要解析本地名称，应该实现/配置什么？
- A. Azure AD 域服务
- B. VPN 网关或 NVA
- C. 作为带有 AD 集成 DNS 的 AD 域控制器的 VM
- D. 使用 AD 域控制器自定义 DNS 设置的 VNet

9. 如果 DNS 服务器不为某个域保存主区域或辅助区域，默认的查找方法是什么？
- A. 转发
- B. 递归查找
- C. 根提示
- D. 有条件转发

10. 创建 DHCP 预留时需要以下哪项？
- A. MAC 地址
- B. IP 地址
- C. FQDN
- D. 计算机名

## Practice test 5 – 管理存储和文件服务

1. 创建 Azure 存储帐户后，需要更改其性能层。可以这样做吗？
- A. 是
- B. 否

2. 创建 Azure VM 后，需要将其磁盘类型从 HDD 更改为 SSD。这可能吗？
- A. 是
- B. 否

3. 创建 VM 后，需要更改 VM 类型以支持高级磁盘。这可能吗？
- A. 是
- B. 否

4. 以下哪种存储身份验证方法支持 NTFS 权限？
- A. 访问密钥
- B. 共享访问签名 (SAS)
- C. 基于 Kerberos 的身份验证

5. 以下哪些是 FSRM 的功能？
- A. 配额管理
- B. 文件筛选管理
- C. 存储报告
- D. 文件分类基础架构
- E. 以上全部

6. 以下哪些是 iSCSI 逻辑组件？
- A. Internet 存储名称服务 (iSNS)
- B. iSCSI 发起程序
- C. iSCSI 限定名称 (IQN)
- D. iSCSI 目标
- E. 以上全部

7. iSCSI 发起程序连接使用哪种身份验证方法？
- A. 密码身份验证协议 (PAP)
- B. 质询握手身份验证协议 (CHAP)
- C. Microsoft 质询握手身份验证协议版本 2 (MS-CHAPv2)
- D. 可扩展身份验证协议 (EAP)

8. 存储空间提供哪些复原级别？
- A. 简单
- B. 基本
- C. 奇偶校验
- D. 镜像

9. 哪些数据卷不能进行重复数据删除处理？
- A. 系统状态文件
- B. 加密文件
- C. 扩展属性文件
- D. 32 KB 或更小的文件
- E. 以上全部

10. 存储副本需要允许哪些通信？
- A. ICMP
- B. SMB (端口 445)
- C. SMB Direct (端口 4445)
- D. WS-MAN (端口 5985)
- E. 以上全部

## 测试答案

### 练习测试 1 – 在本地和云环境中部署和管理 AD DS

- 1. A, B, C.
- 2. A, B, D.
- 3. D.
- 4. B.
- 5. B.
- 6. 错误。正确答案是 gMSAs.
- 7. B.
- 8. 错误。正确答案是必须指定用户帐户。
- 9. C, E.
- 10. 错误。正确答案是必须修改复制计划。

### 练习测试 2 – 管理混合环境中的 Windows 服务器和工作负荷

- 1. B.
- 2. A.
- 3. A.
- 4. C.
- 5. B, C.
- 6. 错误。注意：服务器可以是独立工作组服务器或已加入域。
- 7. B。注意：连接到在 Azure 门户中部署的 WAC 时，需要端口 6516。
- 8. E.
- 9. B.
- 10. C, D.

### 练习测试 3 – 管理虚拟机和容器

- 1. A.
- 2. C.
- 3. A, B.
- 4. B, C, D.
- 5. E.
- 6. A, B, C. 注意：Linux VM 使用 HVC.exe。
- 7. C, D.
- 8. E.
- 9. B.
- 10. C.

### 练习测试 4 – 实现和管理本地和混合网络基础架构

- 1. Azure VPN 网关。注意：Azure 扩展网络不要求您在 Azure 中重新分配 IP 地址。
- 2. B.
- 3. E.
- 4. A.
- 5. C.
- 6. A, B, D.
- 7. A.
- 8. B, C, D.
- 9. C.
- 10. A, B.

### 练习测试 5 – 管理存储和文件服务

- 1. B.
- 2. A.
- 3. A.
- 4. C.
- 5. E.
- 6. A, C.
- 7. B.
- 8. A, C, D.
- 9. E.
- 10. E.

## 摘要

本章涵盖了考试目标中每个关键技能测量部分的考试准备测试。

通过本书所学的知识和技能，您不仅已准备好参加“管理混合核心基础架构的 Windows Server”考试，而且通过本书中的实践练习，您也将有信心并准备好应用这些知识和技能，胜任日常以 Azure 为重点的现实工作角色。

既然这是本书的最后一章，我想借此机会衷心感谢那些通过本内容投入时间和精力，致力于扩展 Azure 和混合 Windows Server 技能与知识的各位读者。我希望这本书能成为跳板，激发您追求进一步培训和教育的兴趣。




## 6 实施和管理 Azure 网络基础设施 179

- Azure 网络服务简介 179
- 在 Azure 中实施 VNet 181
- Azure VNet 181
- 如何实施 IP 寻址 183
- 如何实施 VNet 路由 185
- 在 Azure 中实施网络连接 187
- VPN 188
- ExpressRoute 192
- Azure 网络适配器 193
- Azure 中继 193
- Azure 虚拟 WAN 193
- 在 Azure 中实施网络保护 195
- NSG 195
- ASG 197

- Azure 防火墙 198
- 在 Azure 中实施名称解析 199
- Azure DNS 199
- Azure VM 上的 Windows Server DNS 200
- Azure 中的水平分割 DNS 202
- 实践练习 202
- 入门 203
- 练习 - 实施 Azure VNet 203
- 练习 - 实施 NSG 207
- 练习 - 实施 Azure DNS 224
- 练习 - 实施 Azure VPN 网关 228
- 练习 - 实施 VNet 对等互连 232
- 小结 234
- 建议进一步阅读 235
- 技能检查 235

## 第 3 部分：混合存储

## 7 实施 Windows Server 存储服务 239

- 管理 Windows Server 文件服务器 240
- 什么是 Windows Server 文件系统？240
- 什么是 FAT、FAT32 和 exFAT 文件系统？240
- 什么是 NTFS 文件系统？240
- 什么是 ReFS？241
- 什么是 FSRM？241

- 什么是 SMB？242
- 什么是 VSS？243
- 实施 Windows Server iSCSI 244
- 什么是 iSCSI？244
- iSCSI 组件有哪些？244
- iSCSI 如何工作？246
- 如何实施 iSCSI？247

## 实施存储空间和存储空间直连 249

- 什么是存储空间？249
- 存储空间如何工作？250
- 什么是存储空间直连？252
- 存储空间直连如何工作？253
- 存储空间直连有哪些部署选项？254

## 实施 Windows Server 存储副本 259

- 什么是 Windows Server 存储副本？259
- Windows Server 存储副本如何工作？259
- 如何实施 Windows Server 存储副本？260
- 实施 DFS 261
- 实践练习 262
- 入门 262
- 练习 - 实施数据删除重复 262
- 小结 265
- 建议进一步阅读 265
- 技能检查 266

## 实施 Windows Server 数据删除重复 255

- 什么是 Windows Server 数据删除重复？255
- Windows Server 数据删除重复如何工作？256
- 预计可节省多少磁盘空间？257
- 如何实施 Windows Server 数据删除重复？257

## 8 实施混合文件服务器基础架构 267

- Azure 存储服务简介 267
- 存储类型 268
- 存储帐户 268
- 实施 Azure 文件 270
- 存储层 271
- 存储冗余 272
- 存储加密 273
- 存储保护 274
- 存储网络访问 274
- 存储身份验证 275
- 文件共享权限 278
- Azure 文件同步简介 279

- 将 DFS-R 部署迁移到 Azure 文件同步 280
- 实践练习 281
- 入门 281
- 练习 - 创建存储帐户 281
- 练习 - 创建标准性能 Azure 文件共享 286
- 练习 - 创建高级性能 Azure 文件共享 290
- 小结 293
- 建议进一步阅读 293
- 技能检查 294

## 第 4 部分：混合计算

## 9 在 Windows Server 上实施和管理 Hyper-V 297

- 虚拟化和 Hyper-V 简介 297
- CPU 组 318
- 虚拟机与实体机比较 298
- Hypervisor 调度程序 319
- 什么是 Hyper-V？300
- VM 检查点 320
- 实施和管理 Hyper-V 301
- NIC 联接 320
- Hyper-V 网络 302
- Hyper-V 副本 321
- 嵌套虚拟化 302
- 远程管理 VM 322
- 管理 Hyper-V 303
- 使用受防护结构保护 Hyper-V 工作负载 322
- 创建和管理 Hyper-V 虚拟机 305
- 受防护结构 322
- VM 代际版本 305
- 主机守护者服务 323
- VM 配置版本 306
- 实践练习 324
- VM 设置 307
- 入门 324
- VM 虚拟硬盘格式 310
- 练习 - 使用服务器管理器实施 Hyper-V 角色 325
- VM 磁盘类型 310
- 练习 - 使用 PowerShell 实施 Hyper-V 角色 331
- VM 内存 311
- 练习 - 创建来宾 VM 332
- Hyper-V 集成服务 313
- 小结 336
- 独立设备分配 314
- 建议进一步阅读 336
- 增强会话模式 315
- 技能检查 336
- VM 资源组 318

## 10 实施和管理 Windows Server 容器 339

- 容器简介 340
- Windows Server 容器网络 343
- 容器与 VM 比较 340
- 创建 Windows Server 容器映像 344
- Docker 简介 342
- 保护 Windows Server 容器 342
- 管理 Windows Server 容器映像 345
- 维护容器映像 348
- 实践练习 348
- 入门 348
- 练习 - 使用 PowerShell 在 Windows Server 上安装 Docker 349
- 练习 - 使用 WAC 在 Windows Server 上安装 Docker 350
- 小结 355
- 建议进一步阅读 356
- 技能检查 356

## 11 管理 Windows Server Azure 虚拟机 357

- 管理 Azure VM 数据磁盘 357
- 调整 Azure VM 大小 361
- 为 Azure VM 配置持续交付 363
- 配置到 VM 的连接 365
- WinRM 访问 365
- RDP 访问 366
- 实践练习 367
- 入门 367
- 练习 - 使用 Azure 门户管理数据磁盘 367
- 练习 - 调整 Azure VM 大小 374
- 小结 376
- 建议进一步阅读 376
- 技能检查 377

## 12 在混合环境中管理 Windows Server 379

- 部署 WAC 网关服务器 379
- 脚本部署 380
- 手动部署 381
- 访问 WAC 382
- 在 Azure 门户中部署 WAC 383
- WAC 要求 383
- PowerShell 远程处理 384
- PowerShell 远程处理示例 384
- 第二跳 PowerShell 远程处理 385
- JEA 386
- 实践练习 387
- 入门 388
- 练习 - 在 Azure 门户中实施 WAC 388
- 小结 391
- 建议进一步阅读 392
- 技能检查 392

## 13 使用 Azure 服务管理 Windows Server 393

- 管理 Azure 中 Windows Server VM 的访问权限 394
- 用户访问 394
- 网络访问 399
- 为混合 Windows Server 实施 Azure Arc 399
- 部署先决条件 401
- 网络连接 401
- Azure Policy 来宾配置 401
- 混合 Windows Server 的 Log Analytics 代理 402
- 为混合 Windows Server 实施 Azure 自动化 404
- 简介 404
- Azure 自动化更新管理 404

- 为混合 Windows Server 实施 Microsoft Defender for Cloud 405
- 实践练习 406
- 入门 407
- 练习 - 向 Azure VM 分配访问权限 407
- 练习 - 创建 Azure Policy 分配 413
- 练习 - 设置 Microsoft Defender for Cloud 420
- 练习 - 设置 Microsoft Sentinel 428
- 小结 438
- 建议进一步阅读 438
- 技能检查 439

## 第 5 部分：考试准备

## 14 考试准备模拟测试 443

- 测试问题 443
- 模拟测试 1 - 在本地和云环境中部署和管理 AD DS 443
- 模拟测试 2 - 在混合环境中管理 Windows Server 和工作负载 445
- 模拟测试 3 - 管理虚拟机和容器 447
- 模拟测试 4 - 实施和管理本地和混合网络基础架构 449
- 模拟测试 5 - 管理存储和文件服务 451

- 测试答案 453
- 模拟测试 1 - 在本地和云环境中部署和管理 AD DS 453
- 模拟测试 2 - 在混合环境中管理 Windows Server 和工作负载 454
- 模拟测试 3 - 管理虚拟机和容器 454
- 模拟测试 4 - 实施和管理本地和混合网络基础架构 454
- 模拟测试 5 - 管理存储和文件服务 455

## 小结 455
## 索引 457
## 您可能还喜欢的其他图书 476


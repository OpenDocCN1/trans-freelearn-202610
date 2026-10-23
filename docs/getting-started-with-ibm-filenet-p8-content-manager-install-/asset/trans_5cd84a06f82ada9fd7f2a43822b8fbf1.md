
目录
- 支持的平台 280
- 将内容与工作流集成 281
- 内容存储 281
  - 数据库存储区 282
  - 文件存储区 282
  - 固定内容设备 283
  - 内容联合服务（CFS） 283
- 集群、高可用性和灾难恢复 284
  - 集群 284
  - 高可用性 285
  - 灾难恢复 285
- 分布式部署 286
  - CE 拓扑 287
  - PE 注意事项 288
  - CSE 注意事项 288
  - CE 分布式部署功能 289
    - 内容缓存 289
    - 请求转发 290
  - 典型分布式部署 291
    - 远程应用层 291
    - 远程应用和 CE 层 292
- 总结 292
## 第 10 章：包含和附加组件 293
- 标准 CM 组件 294
  - 服务器组件 294
  - 应用和连接器 295
  - 环境组件 296
- 计划和场景 297
- 合规管理 297
- IBM 企业记录（IER） 298
  - 声明 298
  - 分类 299
  - 保护 299
  - 处置 299
  - 审计和报告 301
- IBM eDiscovery 301
  - IBM eDiscovery Analyzer（eDA） 302
  - IBM eDiscovery Manager（eDM） 303
- IBM Content Collector 303
  - ICC for File Systems 304
  - ICC for Microsoft SharePoint 304
  - ICC for Email 304
  - 任务连接器 304
- IBM Classification Module 305
### 智能归档策略 306
  - 摄取 307
  - 基础设施 307
  - 管理 307
### IBM FileNet Business Process Manager 308
  - IBM FileNet Business Process Framework 308
  - IBM Enterprise Content Management Widgets 309
### 其他组件 309
  - IBM FileNet System Monitor 309
  - IBM FileNet Image Services 309
  - IBM FileNet Capture and Datacap 310
  - Content Management Interoperability Services 310
  - Darwin Information Typing Architecture 311
  - IBM Content Analytics 311
### 总结 312
## 第 11 章：应用开发初探 313
### Content Engine API 314
  - 不要绕过 API 314
    - 读取 314
    - 更新 315
  - API 传输 316
    - CEWS 传输 316
    - EJB 传输 317
    - 用户事务 318
    - JAAS 上下文 319
    - 传输特定编码 319
  - CE .NET 和 Java API 320
  - CEWS 协议 321
  - 附件格式 322
  - 兼容层 323
    - Java 兼容层 324
    - COM 兼容层 325
    - CEWS 3.5 协议 325
### 其他 API 326
  - PE API 326
  - CMIS 326
  - ECM Widgets 327
  - AE/XT 自定义和集成 328
### 开发环境 328
  - Eclipse 中的 CE Java API 329
    - 项目 330
    - CE API 依赖 332
  - 应用代码 333
  - 运行应用 335
  - 我们未展示的一些内容 336
  - VS C# Express 中的 CE .NET API 337
    - 项目 337
    - CE API 依赖 339
    - 应用代码 340
    - 运行应用 342
    - 我们未展示的一些内容 342
  - 总结 342
## 第 12 章：DUCK 示例应用 343
- 业务需求 344
  - 受限数量文档 344
  - 受限流通文档 345
- 终端用户视图设计 345
  - 通用登录屏幕 346
  - 查找文档 347
  - 内容下载 348
  - 添加文档 349
  - 查看文档详情 350
  - 受限数量文档 351
  - 受限流通文档 352
  - 修改文档 352
- 企业要求 354
  - 导航概览 354
- 数据模型和安全模型 356
  - 文档属性 356
  - 限制指示器 357
  - 受限流通 357
  - 访问日志 358
  - 访问控制 359
  - 访问请求 359
  - 受限数量 361
- 技术实现细节 363
  - CE 元数据更改 363
  - 注解子类 364
  - 自定义文档属性 364
  - 自定义事件 364
  - 审计日志配置 365
  - 部署事件处理程序代码模块 366
  - DuckRQB 项目 367
    - 新代码模块 368
    - 修改代码模块 368
    - 调试日志 370
  - Duck 项目 371
  - 导出和部署 Duck.war 374
    - 部署描述符 375
    - 部署到 WAS 376
    - 选择 JSF 运行时 376
    - 配置角色映射 377
    - 启动应用 377
### 总结 378
## 第 13 章：支持、修复包和故障排除 379
### 资源 379
  - 文档 380
    - 信息中心 380
    - 独立文档 381
    - 其他链接 382
    - IBM Redbooks 383
    - IBM developerWorks 384
    - 全球 IBM ECM 社区 384
  - 在线支持资源 385
    - 支持门户 385
    - PMR 和 APAR 385
    - 技术说明 385
    - 个性化通知 386
    - Fix Central 386
  - 按需信息大会 387
### 版本和修复 387
  - 产品发布 387
  - 临时修复 389
  - 修复包 391
  - 安装修复包 393
    - 支持组件 393
    - CM 组件 394
    - CE 4.5.1.1.4 服务器 395
    - CSE 4.5.1.1 客户端 396
    - CSE 4.5.1.1 服务器 397
    - 重新部署更新后的 CE 服务器 398
    - CE 4.5.1.4 客户端 401
    - XT 1.1.4.8 402
### 故障排除 404
  - 预防 404
    - 配置控制 404
    - 快照 405
    - 备份 405
  - 寻找问题 406
    - 初始配置 406
  - 身份验证 407
  - WAS 启动信息 409
  - 性能 409
  - 跟踪日志 410
  - 隔离 410
  - 数据库调优 411
### 总结 412
### 索引 413

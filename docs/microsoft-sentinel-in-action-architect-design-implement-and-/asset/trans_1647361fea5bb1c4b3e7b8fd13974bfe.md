

# Microsoft Sentinel
实战

# 第二版

将 Microsoft Sentinel 架构、设计、实施和运营作为您安全解决方案的核心

# Microsoft Sentinel
实战
*第二版*

将 Microsoft Sentinel 架构、设计、实施和运营作为您安全解决方案的核心

Richard Diver

Gary Bushey

John Perkins

Packt
伯明翰—孟买

# Microsoft Sentinel 实战

# 第二版

版权所有 © 2022 Packt Publishing

保留所有权利。在未获得出版商事先书面许可的情况下，本书的任何部分不得以任何形式或任何手段复制、存储在检索系统中或传播，但嵌入在评论或评论文章中的简短引文除外。

在编写本书时已尽一切努力确保所提供信息的准确性。然而，本书中包含的信息按“现状”出售，不提供任何明示或暗示的保证。作者、Packt Publishing 及其经销商和分销商均不对因本书直接或间接造成的任何损害承担责任。

Packt Publishing 已尽力就本书中提及的所有公司和产品提供商标信息，并以适当的方式使用大写字母。然而，Packt Publishing 不能保证此信息的准确性。

产品组经理：Vijin Boricha

出版产品经理：Meeta Rajani

高级编辑：Arun Nadar

内容开发编辑：Sulagna Mohanty

技术编辑：Arjun Varma

校对编辑：Safis Editing

项目协调员：Shagun Saini

校对：Safis Editing

索引：Vinayak Purushotham

制作设计师：Vijay Kamble

首次出版：2020 年 5 月

第二版：2022 年 1 月

制作参考：1021221

Packt Publishing Ltd 出版

Livery Place

35 Livery Street

Birmingham

B3 2PB, UK.

ISBN 978-1-80181-553-6

www.packt.com

## 贡献者

## 关于作者

**Richard Diver** 是 Microsoft 安全解决方案组的资深技术业务战略经理，专注于开发安全合作伙伴。总部位于芝加哥，Richard 与高级安全和合规合作伙伴合作，帮助他们在整个 Microsoft 平台上构建解决方案，包括 Microsoft Sentinel、Microsoft Defender、Microsoft 365 安全解决方案等。在加入 Microsoft 之前，Richard 在多个行业和多家 Microsoft 合作伙伴工作，为全球各地的各种客户设计和实施云安全解决方案。他所有的空闲时间通常都花在家人身上。

**Gary Bushey** 是一位拥有超过 25 年 IT 经验的 Azure 安全专家。他很早就开始了自己的职业生涯，当时他在五年级时帮助数学老师完成编程作业，并工作了一个整个夏天来负担得起他的第一台电脑，一台 Commodore 64。当他 14 岁时出售了他的第一个程序——一套公寓管理系统后，他就上瘾了。在他的职业生涯中，他曾担任开发人员、顾问、培训师和架构师。当不花时间在电脑前时，你会发现他在树林里徒步旅行、拍照，或只是随意选一个方向，看看下一个拐角有什么。

**John Perkins** 是 Threat Angler 的创始人兼负责人，这是一家网络安全服务提供商，专注于托管服务、专业服务和培训，重点是为各种规模的客户提供网络安全成果。John 在网络安全领域拥有超过 20 年的经验，并在职业生涯中涉足了几乎所有网络安全学科。他拥有使用 Microsoft Sentinel 等众多应用的经验，并为多家大型服务提供商设计、构建和领导托管安全服务。在业余时间，John 喜欢与家人共度时光、旅行和保持活跃。

## 关于审阅者

**Ashwin Patil** 目前担任 **Microsoft Threat Intelligence Center (MSTIC)** 的高级项目经理，拥有超过 10 年完全专注于安全监控和事件响应的经验，负责防御企业网络。在他目前的角色中，他主要从事威胁狩猎、为 Microsoft Sentinel 在 **Kusto Query Language (KQL)** 中的检测研究，以及开发用 Python/R 编写的 Jupyter 笔记本，用于在各种云和本地安全事件日志数据源中进行威胁狩猎和调查。他拥有计算机工程学士学位，并拥有多个 SANS 认证，包括 GCIA、GCFE 和 GCIH，专业领域为 **Digital Forensics and Incident Response (DFIR)**。

**Dennis Pike** 是 Island 的原始销售工程师，这是一家隐秘模式的安全初创公司。他会告诉你更多，但他们可能会把他放到一个只有排球的孤岛上。他出生于肯塔基州，令人惊讶的是他不喜欢波旁威士忌，反而成为了一名全国排名的啤酒评委。他拥有弗吉尼亚大学系统工程学士学位，并在过去 25 年里从事 IT 工作，包括担任 Microsoft 的全球黑带 – 高级安全分析师，专注于 Microsoft Sentinel。

> *我想感谢我的妻子 Heather 的耐心、爱和支持。*

**Rod Trent** 是 Microsoft 的高级云安全倡导者，也是 Microsoft Sentinel 全球 SME，帮助客户从现有 SIEM 迁移到 Microsoft Sentinel，以实现通过提高效率而实现更好安全且不妥协的承诺。他是一位丈夫、父亲和首次成为祖父的人，所以请慢慢说大声点。他在业余时间（如果真的存在的话）同时观看 *Six Million Dollar Man* 系列剧集并编写 KQL 查询。

# 4

# 将威胁情报与 Microsoft Sentinel 集成

威胁情报简介 92

了解 STIX 和 TAXII 94

为您的需求选择合适的情报源 95

实施 TI 连接器 96

启用数据连接器 96

在 Azure AD 中注册应用 98

配置 MineMeld TI 源 104

确认数据正在被摄取以供 Microsoft Sentinel 使用 113

总结 116

问题 116

延伸阅读 116

# 5

# 使用 Kusto 查询语言 (KQL)

运行 KQL 查询 118

KQL 命令简介 120

表格运算符 122

查询语句 139

let 语句 139

标量函数 140

ago() 函数 140

字符串运算符 140

总结 142

问题 142

延伸阅读 142

# 6

# Microsoft Sentinel 日志和编写查询

Microsoft Sentinel 日志页面简介 144

浏览日志页面 144

页面标题 145

Tables 面板 155

Queries 面板 159

Functions 面板 159

Filter 面板 160

KQL 代码窗口 164

运行查询 168

结果窗口 168

了解更多 176

审查替代存储选项 87

总结 89

问题 89

延伸阅读 89

# 第 3 部分：安全威胁狩猎

# 7

# 创建分析规则

Microsoft Sentinel Analytics 简介 185

分析规则类型 186

浏览 Analytics 主页 188

创建分析规则 197

从规则模板创建规则 197

使用向导创建新规则 198

管理分析规则 218

总结 219

问题 220

延伸阅读 220

# 8

# 创建和使用工作簿

工作簿页面概述 222

工作簿标题 223

Templates 视图 224

工作簿详细视图 225

缺少所需数据类型 225

已保存模板按钮 226

浏览现有工作簿 228

创建工作簿 230

使用模板创建工作簿 231

从头创建新工作簿 232

编辑工作簿 234

高级编辑 237

管理工作簿 239

工作簿步骤类型 241

文本 242

查询 243

指标 247

参数 248

链接/选项卡 254

组 258

高级设置 260

样式 264

总结 265

问题 265

延伸阅读 266

编写查询 176

计费数据摄取 177

登录的地图视图 178

其他有用的表 179

总结 180

问题 181

延伸阅读 181


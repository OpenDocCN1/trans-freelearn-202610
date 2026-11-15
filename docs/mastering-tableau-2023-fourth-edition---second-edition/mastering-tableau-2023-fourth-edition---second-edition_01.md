# 序言

## Christina Stathopoulos 作序

在历史的大部分时期，我们都因缺乏信息或数据而苦恼，无法借此理解我们的世界。即使随着计算机和互联网的出现，很长一段时间里，我们仍依赖抽样和小数据集来估算大趋势。但此类估算伴随着风险，首当其冲的就是高度的不确定性。如今，形势逆转，情况恰恰相反——我们拥有过多的信息、过多的数据，正淹没在数字内容的汪洋大海中。大数据集意味着不再需要估算。我们触手可及地掌握了对实际总体趋势的深入理解，但当试图从过剩的数据中挖掘有意义的模式和洞见时，这可能会让人不知所措。由于技术进步和社会变革的交汇，特别是互联网和智能手机使用的全球增长，我们掌握的数据比我们懂得如何处理的还要多。随着世界变得日益互联，线上世界与线下世界的融合程度加深，这种数据激增预计将进一步加速。在这些变化中，企业发现自己被迫理解手头的所有数据，努力学习如何将数据作为其差异化因素，但我们反复自问的问题是：“*怎么做？*”

恰当的商业智能和分析技术就是答案。如果做得正确，数据可以逐步被提炼为有价值的趋势和洞见。Tableau 是我所谓的“数据解谜”领域的市场领导者。凭借其直观的界面和强大的功能，Tableau 让个人和组织都能将复杂的数据集转化为可执行的智能。在《精通 Tableau 2023》中，作者兼知名 Tableau 专家 Marleen Meier 将带您了解实施 Tableau 高级功能所需的一切知识。

我最近在她目前的居住地新加坡与 Marleen 共度时光，除其他数据话题外，我们还讨论了她关于《精通 Tableau》最新版的工作。我对她对该工具的热情以及她致力于让出版物紧跟最新功能和能力的决心印象深刻。考虑到数据领域持续快速变化，而 Tableau 也随之调整和扩展以满足市场需求，这绝非易事。本书在商业智能领域出色地应对了最新的 Tableau 产品。从封面到封底，浓缩在 16 章中，您将通过可视化解释和动手实践的真实案例，掌握 Tableau 的基础主题。您将被引导使用 Tableau Server、Prep 和 Desktop 中的全新功能，并通过 Python 和 R 集成、地理空间分析（这是我个人最喜欢的，因为我曾在 Waze 从事这方面的工作！）、时间序列分析等高级功能接受挑战。

作为一名同行数据专业人士、讲师和该领域的领军声音，分析，尤其是可视化分析，对我而言至关重要。亲身体验低代码和无代码工具普及数据使用的进程，令人着迷。仅需几次点击或简单的拖放操作，您今天就能用 Tableau 完成令人难以置信的事情，而这些任务在过去需要大量代码和一个开发团队。我想不出比这本书更好的掌握 Tableau 的方式了。正如您即将发现的，作者 Marleen Meier 通过详细阐述每个概念——从基础仪表板到高级机器学习技术——帮助简化了一个人充分利用 Tableau 潜力的能力。在接下来的页面中，您将作为高级用户更自如地驾驭 Tableau。希望您像我一样享受这本书，并在未来创建和开发自己的 Tableau 项目时，将这宝贵的资源留在身边。

*Christina Stathopoulos*

*Dare to Data 创始人*

## [Author Name] 作序

我想知道您通往这本书、通往 Tableau 的旅程是怎样的？您是否被要求用数据做些什么？构建仪表板？设计信息图表？访问工作中的数据库？监控业务 KPI？发掘多个数据集及其变量之间的联系？也许您是一名想了解如何做到以上所有的学生，正在通往资格认证的道路上，又或许您是正在寻找新工作或新职业的个人？

更多地了解数据是许多新旧职业机会的基石。而且这不仅关乎工作：通过数据收集和连接人一直很重要。甚至在算盘、纸张，随后是计算机接管从应用 A 到应用 B 的流程管理（这对商业、体育、社交以及实际上整个现代生活都至关重要）很久以前，我们的石器时代祖先就已在使用计数棒来记录和共享信息。

移动数据（及其所代表的信息）只是问题的一半。以有益于他人的方式共享数据是另一半。获益通常需要丰富其应用所在的人文背景。面对不断变化的现代数据基础设施，寻找背景并务实地交付价值，在我看来，是人们转向 Tableau 的最大原因。

我自己的数据之旅始于 AI 和机器学习，随后是跨数百个网站改善客户体验。在那些 Dotcom 年代，我花费大量时间处理业务问题，将其转化为数据问题，然后寻找针对不同企业的不同解决方案。这意味着在直接根据这些发现采取行动，或以鼓励他人采取行动的方式呈现它们之前，先熟悉数据、数据结构和流程，以及用户界面和体验。




# 序言

初次使用 Tableau 时，显而易见，它的设计初衷是“帮助人们查看和理解数据”，这正是 Tableau 的企业座右铭。它不仅仅是数据流向的可视化终点（正如许多认为 Tableau “只是另一个仪表板工具”的人所理解的那样）。它的设计旨在鼓励与数据交互，并让人们找到新问题的答案，而不仅仅是作为一个供观察的静态表格可视化。它利用了许多视觉感知原理，鼓励人们发现自己的模式、观察自己的异常值，并用自己的眼睛查看存储在任何位置的底层数据，而无需了解编码或现代数据库管理知识。

不久后，我认识了本书作者 Marleen。Marleen 正在弥合深耕数据/数据科学的人员与其业务合作伙伴之间的鸿沟。面临的几个挑战是：“如何让业务团队更贴近数据科学团队，并最终贴近业务数据？如何以业务合作伙伴能够理解和应用的方式提供 AI 结果和模型的背景？”显然，Marleen 能够胜任弥合这一鸿沟的任务，而借助 Tableau，她找到了一款能够实现这一目标的工具。

现在，我们有了这本书可以共同阅读，并在未来作为参考。无论你打算如何使用它，无论你希望达成什么成果，这本书都将帮助你掌握 Tableau，更重要的是，掌握让任何数字数据对我们这些“模拟人类”更加有效的艺术。本书提供了关于数据访问和处理等线性任务的建议，并延伸至极其非线性的问题解决过程，以及如何有效地与他人沟通数据。

面对新问题时，往往不清楚从何入手。我应该采取的第一步是什么？首先定义问题总是最好的，紧接着会问：“但数据表明了什么？”随着你从现有数据所展示的世界中了解更多，最初的问题陈述往往会发生变化。本书中的方法无论你使用什么工具都将对你大有裨益，而在使用 Tableau 时，你会体会到在解决问题和数据沟通中所需的敏捷性。

通过阅读本书，我期望你将重新认识如何发掘解决方案，以新获得的信心逐步攻克数据任务，并准备好以同事、朋友和家人能够理解的可视化方式展示你的发现。欢迎阅读 *《精通 Tableau》*。

*David Sigerson*

*Tableau 区域现场首席技术官*

# 贡献者

# 关于作者

Marleen Meier 是一位成就卓著的分析师和作家，对统计学和数据充满热情。Marleen 运用传统方法论和机器学习、AI 等更现代的方法，致力于挖掘有意义的洞见。

目前担任 ABN AMRO Clearing 的亚太区数据卓越中心负责人，Marleen 处于创新前沿，在全球金融环境中实施数据驱动策略。Marleen 专注于风险分析、数据可视化、数据质量和数据管控，不断挑战现状，重新定义最佳实践。她曾在德国、荷兰、美国和新加坡等多个国家生活和工作，这使她能为工作带来多元化和全球化的视角。

Marleen 的奉献精神、专业知识和拓展数据分析边界的承诺，使她成为该领域的权威专家。通过写作和演讲，她旨在赋能个人和组织，释放其数据资产的全部潜力。

纪念 Leo。我们非常想念你。

# 关于审阅者

Ruchi Arora 是一位技术和数据高管，拥有转型和扩展业务的丰富经验。她拥有独特的问题解决能力，并在 AI/ML、分析和数据产品工作方面经验丰富，持续实现营收增长、提高运营效率并让客户满意。

Ruchi 以软件开发人员身份起步，后转向 AI 数据管理和分析领域。她曾担任多家科技巨头和财富 500 强企业的数据负责人。Ruchi 也是一位知名演讲者。她以解决复杂业务问题和协助行业完成数字化转型、RPA 和云迁移为荣。

此外，Ruchi 是多元化、公平性与包容性的倡导者，也是众多科技和数据领域女性社区的活跃成员。

Ruchi 是 Women in Data 的志愿者，负责西雅图分会，同时也是 Bellevue Children’s Academy 家长协会董事会主席。

Ruchi 毕业于哈佛商学院领导力与综合管理项目，并拥有计算机应用硕士学位。

Jayen Thakker 在分析领域拥有 18 年经验，是 Tableau 和 Cognos Analytics 的资深专家。他亲自为客户和内部团队举办了近 100 场关于这些平台的培训，展示了其专业知识，进一步巩固了其作为行业受信任领导者的地位。Jayen 擅长获取新客户和推动追加销售，同时担任企业解决方案架构师，为企业打造高效的分析解决方案。作为 BI 实践经理，他领导一个由 80 多名专业人士组成的精干团队，在 BI 和数据仓库领域成功交付了数百万美元的项目。凭借对卓越的追求、对创新的热情和对知识分享的承诺，Jayen 持续在分析行业产生重大影响。

我想借此机会感谢我挚爱的家人在我审阅本书期间（以及其他时候）给予我的支持。他们是我力量和耐心的源泉。谢谢你们，亲爱的家人。

还有一位相信我能胜任本书审阅工作的人是 Muthukumar S M，他引荐我获得此机会，并从一开始就相信我。非常感谢你，Muthu。

# 在 Discord 上了解更多

扫描下方二维码，加入本书的 Discord 社区——你可以在那里分享反馈、向作者提问、了解新版本发布：

`https://packt.link/tableau`

![](img/QR_Code2044012316175764640.png)

# 第 8 章

## 3. 帕累托图 [`#Chapter_8.xhtml#_idParaDest-142`]

### 1. 使用帕累托图 [`#Chapter_8.xhtml#_idParaDest-143`]

### 2. 帕累托图 – 进阶 [`#Chapter_8.xhtml#_idParaDest-144`]

## 4. 自定义背景图片 [`#Chapter_8.xhtml#_idParaDest-145`]

### 1. 创建自定义多边形 [`#Chapter_8.xhtml#_idParaDest-146`]

#### 1. 在零度岛周围绘制正方形 [`#Chapter_8.xhtml#_idParaDest-147`]

#### 2. 使用多边形创建交互式书架 [`#Chapter_8.xhtml#_idParaDest-148`]

### 2. 在 Tableau 中分析一局棋 [`#Chapter_8.xhtml#_idParaDest-149`]

#### 1. 在 Tableau 中创建 SVG 文件 [`#Chapter_8.xhtml#_idParaDest-150`]

#### 2. 创建网格 [`#Chapter_8.xhtml#_idParaDest-151`]

#### 3. 使用网格生成数据集 [`#Chapter_8.xhtml#_idParaDest-152`]

#### 4. 可视化棋局 [`#Chapter_8.xhtml#_idParaDest-153`]

### 3. 在背景图片上创建多边形 [`#Chapter_8.xhtml#_idParaDest-154`]

## 5. Tableau Exchange [`#Chapter_8.xhtml#_idParaDest-155`]

### 1. Tableau 扩展程序 [`#Chapter_8.xhtml#_idParaDest-156`]

### 2. 使用扩展程序 [`#Chapter_8.xhtml#_idParaDest-157`]

#### 1. 加速器 [`#Chapter_8.xhtml#_idParaDest-158`]

#### 2. 连接器 [`#Chapter_8.xhtml#_idParaDest-159`]

## 6. Einstein Discovery [`#Chapter_8.xhtml#_idParaDest-160`]

## 7. 总结 [`#Chapter_8.xhtml#_idParaDest-161`]

# 第 9 章：使用地图 [`#Chapter_9.xhtml#_idParaDest-162`]

## 1. 在不离开 Tableau 的情况下扩展 Tableau 的地图功能 [`#Chapter_9.xhtml#_idParaDest-163`]

## 2. 创建自定义多边形 [`#Chapter_9.xhtml#_idParaDest-164`]

### 1. 德克萨斯州的多边形 [`#Chapter_9.xhtml#_idParaDest-165`]

## 3. 热力图 [`#Chapter_9.xhtml#_idParaDest-166`]

## 4. 双轴和地图分层 [`#Chapter_9.xhtml#_idParaDest-167`]

### 1. 使用双轴 [`#Chapter_9.xhtml#_idParaDest-168`]

### 2. 添加地图图层 [`#Chapter_9.xhtml#_idParaDest-169`]

## 5. 使用其他技术扩展 Tableau 制图 [`#Chapter_9.xhtml#_idParaDest-170`]

### 1. 将 WMS 与自定义地图配合使用 [`#Chapter_9.xhtml#_idParaDest-171`]

### 2. 探索 Mapbox [`#Chapter_9.xhtml#_idParaDest-172`]

## 6. 替换地图 [`#Chapter_9.xhtml#_idParaDest-173`]

## 7. 自定义地理编码 [`#Chapter_9.xhtml#_idParaDest-174`]

## 8. 总结 [`#Chapter_9.xhtml#_idParaDest-175`]

# 第 10 章：使用 Tableau 进行演示 [`#Chapter_10.xhtml#_idParaDest-176`]

## 1. 从 Tableau 导出最佳图像 [`#Chapter_10.xhtml#_idParaDest-177`]

### 1. Tableau 原生导出功能 [`#Chapter_10.xhtml#_idParaDest-178`]

## 2. 从 Tableau 到 PowerPoint [`#Chapter_10.xhtml#_idParaDest-179`]

### 1. 创建模板 [`#Chapter_10.xhtml#_idParaDest-180`]

### 2. 创建用于打印的仪表板 [`#Chapter_10.xhtml#_idParaDest-181`]

### 3. 半自动化 PowerPoint 演示文稿 [`#Chapter_10.xhtml#_idParaDest-182`]

## 3. 将 Tableau 嵌入 PowerPoint [`#Chapter_10.xhtml#_idParaDest-183`]

## 4. 将 Tableau 嵌入 Google Slides [`#Chapter_10.xhtml#_idParaDest-184`]

## 5. Tableau 动画 [`#Chapter_10.xhtml#_idParaDest-185`]

### 1. 使用动画导出多张图像 [`#Chapter_10.xhtml#_idParaDest-186`]

### 2. 在 Tableau 中使用动画以在 PowerPoint 中创建动画 [`#Chapter_10.xhtml#_idParaDest-187`]

## 6. 用于演示的故事点和仪表板 [`#Chapter_10.xhtml#_idParaDest-188`]

## 7. 演示资源 [`#Chapter_10.xhtml#_idParaDest-189`]

## 8. 总结 [`#Chapter_10.xhtml#_idParaDest-190`]

# 第 11 章：设计仪表板和可视化最佳实践 [`#Chapter_11.xhtml#_idParaDest-191`]

## 1. 可视化设计理论 [`#Chapter_11.xhtml#_idParaDest-192`]

## 2. 格式规则 [`#Chapter_11.xhtml#_idParaDest-193`]

### 1. 保持字体选择简单 [`#Chapter_11.xhtml#_idParaDest-194`]

### 2. 按可见性顺序使用线条 [`#Chapter_11.xhtml#_idParaDest-195`]

### 3. 以三到五个为一组使用色带 [`#Chapter_11.xhtml#_idParaDest-196`]

## 3. 颜色规则 [`#Chapter_11.xhtml#_idParaDest-197`]

### 1. 保持颜色简单且有限 [`#Chapter_11.xhtml#_idParaDest-198`]

### 2. 尊重颜色的心理暗示 [`#Chapter_11.xhtml#_idParaDest-199`]

### 3. 适配色盲用户 [`#Chapter_11.xhtml#_idParaDest-200`]

### 4. 少用纯色 [`#Chapter_11.xhtml#_idParaDest-201`]

### 5. 选择颜色变化而非符号变化 [`#Chapter_11.xhtml#_idParaDest-202`]

## 4. 可视化类型规则 [`#Chapter_11.xhtml#_idParaDest-203`]

### 1. 保持形状简单 [`#Chapter_11.xhtml#_idParaDest-204`]

### 2. 少用饼图 [`#Chapter_11.xhtml#_idParaDest-205`]

### 3. 使仪表板简单且健壮 [`#Chapter_11.xhtml#_idParaDest-206`]

### 4. 良好地呈现密集信息 [`#Chapter_11.xhtml#_idParaDest-207`]

### 5. 讲述故事 [`#Chapter_11.xhtml#_idParaDest-208`]

### 6. 在仪表板上最大化文档 [`#Chapter_11.xhtml#_idParaDest-209`]

#### 1. 可视化类型 [`#Chapter_11.xhtml#_idParaDest-210`]

## 5. 保持可视化简单 [`#Chapter_11.xhtml#_idParaDest-211`]

## 6. 仪表板设计 [`#Chapter_11.xhtml#_idParaDest-212`]

### 1. 仪表板布局 [`#Chapter_11.xhtml#_idParaDest-213`]

#### 1. 黄金比例布局 [`#Chapter_11.xhtml#_idParaDest-214`]

#### 2. 四象限布局 [`#Chapter_11.xhtml#_idParaDest-215`]

#### 3. 小倍数布局 [`#Chapter_11.xhtml#_idParaDest-216`]

### 2. 利用工作表交换 [`#Chapter_11.xhtml#_idParaDest-217`]

### 3. 创建可折叠菜单 [`#Chapter_11.xhtml#_idParaDest-218`]

## 7. 仪表板最佳实践 [`#Chapter_11.xhtml#_idParaDest-219`]

### 1. 动作 [`#Chapter_11.xhtml#_idParaDest-220`]

#### 1. 筛选动作 [`#Chapter_11.xhtml#_idParaDest-221`]

#### 2. 高亮动作 [`#Chapter_11.xhtml#_idParaDest-222`]

#### 3. URL 动作 [`#Chapter_11.xhtml#_idParaDest-223`]

#### 4. 导航动作 [`#Chapter_11.xhtml#_idParaDest-224`]

#### 5. 参数动作 [`#Chapter_11.xhtml#_idParaDest-225`]

#### 6. 集合动作 [`#Chapter_11.xhtml#_idParaDest-226`]

### 2. 下载 [`#Chapter_11.xhtml#_idParaDest-227`]

### 3. 项目层级 [`#Chapter_11.xhtml#_idParaDest-228`]

### 4. 用于 [`#Chapter_11.xhtml#_idParaDest-229`]

#### 1. 额外内容 [`#Chapter_11.xhtml#_idParaDest-230`]

#### 2. 什么有效？ [`#Chapter_11.xhtml#_idParaDest-231`]

#### 3. 为什么效果更好 [`#Chapter_11.xhtml#_idParaDest-232`]

#### 4. 什么有效？ [`#Chapter_11.xhtml#_idParaDest-233`]

#### 5. 为什么效果更好 [`#Chapter_11.xhtml#_idParaDest-234`]

#### 6. 为什么效果更好 [`#Chapter_11.xhtml#_idParaDest-235`]

## 8. 总结 [`#Chapter_11.xhtml#_idParaDest-236`]

# 第 12 章：利用高级分析 [`#Chapter_12.xhtml#_idParaDest-237`]

## 1. 可视化世界指数相关性 [`#Chapter_12.xhtml#_idParaDest-238`]

### 1. 绘制散点图 [`#Chapter_12.xhtml#_idParaDest-239`]

### 2. 添加轴分布 [`#Chapter_12.xhtml#_idParaDest-240`]

### 3. 添加相关性矩阵 [`#Chapter_12.xhtml#_idParaDest-241`]

### 4. 完成仪表板 [`#Chapter_12.xhtml#_idParaDest-242`]

## 2. 使用芝加哥交通违章数据进行地理空间分析 [`#Chapter_12.xhtml#_idParaDest-243`]

### 1. 准备数据 [`#Chapter_12.xhtml#_idParaDest-244`]

## 3. 构建路口地图 [`#Chapter_12.xhtml#_idParaDest-245`]

### 1. 添加相应的热力图工作表 [`#Chapter_12.xhtml#_idParaDest-246`]



# 目录

## 第 12 章

### 2. 完成仪表板
`#Chapter_12.xhtml#_idParaDest-247`

### 4. 使用距离度量扩展地理空间分析
`#Chapter_12.xhtml#_idParaDest-248`

#### 1. 向地图添加度量点
`#Chapter_12.xhtml#_idParaDest-249`

#### 2. 添加距离线
`#Chapter_12.xhtml#_idParaDest-250`

### 5. 摘要
`#Chapter_12.xhtml#_idParaDest-251`

## 第 13 章 提升性能
`#Chapter_13.xhtml#_idParaDest-252`

### 1. 理解性能记录仪表板
`#Chapter_13.xhtml#_idParaDest-253`

### 2. 硬件和即时技术
`#Chapter_13.xhtml#_idParaDest-254`

#### 1. “运行更新”功能
`#Chapter_13.xhtml#_idParaDest-255`

#### 2. 小型提取
`#Chapter_13.xhtml#_idParaDest-256`

### 3. 连接到数据源
`#Chapter_13.xhtml#_idParaDest-257`

#### 1. 高效处理大型数据源
`#Chapter_13.xhtml#_idParaDest-258`

##### 1. 定义主键和外键
`#Chapter_13.xhtml#_idParaDest-259`

##### 2. 将列定义为 NOT NULL
`#Chapter_13.xhtml#_idParaDest-260`

##### 3. 索引
`#Chapter_13.xhtml#_idParaDest-261`

### 4. 使用提取
`#Chapter_13.xhtml#_idParaDest-262`

#### 1. 构建提取
`#Chapter_13.xhtml#_idParaDest-263`

#### 2. 聚合
`#Chapter_13.xhtml#_idParaDest-264`

#### 3. 优化提取
`#Chapter_13.xhtml#_idParaDest-265`

### 5. 明智地使用筛选器
`#Chapter_13.xhtml#_idParaDest-266`

#### 1. 提取筛选器
`#Chapter_13.xhtml#_idParaDest-267`

#### 2. 数据源筛选器
`#Chapter_13.xhtml#_idParaDest-268`

#### 3. 上下文筛选器
`#Chapter_13.xhtml#_idParaDest-269`

#### 4. 维度和度量筛选器
`#Chapter_13.xhtml#_idParaDest-270`

#### 5. 表计算筛选器
`#Chapter_13.xhtml#_idParaDest-271`

#### 6. 使用操作代替筛选器
`#Chapter_13.xhtml#_idParaDest-272`

### 6. 高效计算
`#Chapter_13.xhtml#_idParaDest-273`

#### 1. 优先考虑代码值
`#Chapter_13.xhtml#_idParaDest-274`

#### 2. 详细级别计算或表计算
`#Chapter_13.xhtml#_idParaDest-275`

### 7. 提升性能的其他方法
`#Chapter_13.xhtml#_idParaDest-276`

#### 1. 避免仪表板过于拥挤
`#Chapter_13.xhtml#_idParaDest-277`

#### 2. 固定仪表板大小
`#Chapter_13.xhtml#_idParaDest-278`

#### 3. 使用 Tableau Prep Builder
`#Chapter_13.xhtml#_idParaDest-279`

#### 4. 设定预期
`#Chapter_13.xhtml#_idParaDest-280`

#### 5. 工作簿优化器
`#Chapter_13.xhtml#_idParaDest-281`

### 8. 摘要
`#Chapter_13.xhtml#_idParaDest-282`

## 第 14 章 探索 Tableau Server 和 Tableau Cloud
`#Chapter_14.xhtml#_idParaDest-283`

### 1. 将数据源发布到 Tableau Server
`#Chapter_14.xhtml#_idParaDest-284`

#### 1. Tableau 文件类型
`#Chapter_14.xhtml#_idParaDest-285`

##### 1. Tableau 数据源
`#Chapter_14.xhtml#_idParaDest-286`

##### 2. Tableau 打包数据源
`#Chapter_14.xhtml#_idParaDest-287`

##### 3. Tableau 工作簿
`#Chapter_14.xhtml#_idParaDest-288`

##### 4. Tableau 打包工作簿
`#Chapter_14.xhtml#_idParaDest-289`

##### 5. 其他文件类型
`#Chapter_14.xhtml#_idParaDest-290`

### 2. Web 创作
`#Chapter_14.xhtml#_idParaDest-291`

#### 1. 使用 Web 创作编辑现有工作簿
`#Chapter_14.xhtml#_idParaDest-292`

#### 2. 了解 Tableau Server Web 创作环境
`#Chapter_14.xhtml#_idParaDest-293`

### 3. 在 Tableau Server 上维护工作簿
`#Chapter_14.xhtml#_idParaDest-294`

#### 1. 修订历史
`#Chapter_14.xhtml#_idParaDest-295`

#### 2. 用户筛选器
`#Chapter_14.xhtml#_idParaDest-296`

### 4. 更多 Tableau Server 设置和功能
`#Chapter_14.xhtml#_idParaDest-297`

#### 1. 工作表级别的功能
`#Chapter_14.xhtml#_idParaDest-298`

#### 2. 视图级别的功能
`#Chapter_14.xhtml#_idParaDest-299`

### 5. 摘要
`#Chapter_14.xhtml#_idParaDest-300`

## 第 15 章 集成编程语言
`#Chapter_15.xhtml#_idParaDest-301`

### 1. 集成编程语言
`#Chapter_15.xhtml#_idParaDest-302`

### 2. R 安装与集成
`#Chapter_15.xhtml#_idParaDest-303`

### 3. 实现 R 功能
`#Chapter_15.xhtml#_idParaDest-304`

#### 1. 在 R 中复现 Tableau 原生功能
`#Chapter_15.xhtml#_idParaDest-305`

#### 2. 使用 R 进行回归计算
`#Chapter_15.xhtml#_idParaDest-306`

#### 3. 在 Tableau 中使用 R 进行聚类
`#Chapter_15.xhtml#_idParaDest-307`

#### 4. 引入分位数
`#Chapter_15.xhtml#_idParaDest-308`

#### 5. 性能挑战
`#Chapter_15.xhtml#_idParaDest-309`

### 4. Python 安装与集成
`#Chapter_15.xhtml#_idParaDest-310`

### 5. 实现 Python 功能
`#Chapter_15.xhtml#_idParaDest-311`

#### 1. 随机数和正态随机数
`#Chapter_15.xhtml#_idParaDest-312`

##### 1. 生成随机数
`#Chapter_15.xhtml#_idParaDest-313`

##### 2. 正态随机数
`#Chapter_15.xhtml#_idParaDest-314`

#### 2. 计算情感分析
`#Chapter_15.xhtml#_idParaDest-315`

#### 3. 使用 TabPy 部署模型
`#Chapter_15.xhtml#_idParaDest-316`

#### 4. 预部署的 TabPy 函数
`#Chapter_15.xhtml#_idParaDest-317`

#### 5. 磨练你的 R 和 Python 技能
`#Chapter_15.xhtml#_idParaDest-318`

#### 6. Einstein Discovery
`#Chapter_15.xhtml#_idParaDest-319`

### 6. 摘要
`#Chapter_15.xhtml#_idParaDest-320`

## 第 16 章 制定数据治理实践
`#Chapter_16.xhtml#_idParaDest-321`

### 1. 什么是数据治理？
`#Chapter_16.xhtml#_idParaDest-322`

### 2. 数据治理原则
`#Chapter_16.xhtml#_idParaDest-323`

#### 1. 合法性、公平性和透明度
`#Chapter_16.xhtml#_idParaDest-324`

#### 2. 目的限制
`#Chapter_16.xhtml#_idParaDest-325`

#### 3. 数据最小化
`#Chapter_16.xhtml#_idParaDest-326`

#### 4. 准确性
`#Chapter_16.xhtml#_idParaDest-327`

#### 5. 存储限制
`#Chapter_16.xhtml#_idParaDest-328`

#### 6. 完整性和保密性
`#Chapter_16.xhtml#_idParaDest-329`

#### 7. 问责制
`#Chapter_16.xhtml#_idParaDest-330`

### 3. Tableau 中的数据治理
`#Chapter_16.xhtml#_idParaDest-331`

#### 1. 数据源连接
`#Chapter_16.xhtml#_idParaDest-332`

#### 2. 数据访问控制
`#Chapter_16.xhtml#_idParaDest-333`

#### 3. 数据安全
`#Chapter_16.xhtml#_idParaDest-334`

#### 4. 元数据管理
`#Chapter_16.xhtml#_idParaDest-335`

#### 5. 数据认证
`#Chapter_16.xhtml#_idParaDest-336`

#### 6. 版本控制
`#Chapter_16.xhtml#_idParaDest-337`

#### 7. 监控和审计
`#Chapter_16.xhtml#_idParaDest-338`

#### 8. 协作和文档
`#Chapter_16.xhtml#_idParaDest-339`

### 4. 实操示例
`#Chapter_16.xhtml#_idParaDest-340`

#### 1. 认证数据源
`#Chapter_16.xhtml#_idParaDest-341`

#### 2. 数据质量警告
`#Chapter_16.xhtml#_idParaDest-342`

#### 3. Tableau 血缘
`#Chapter_16.xhtml#_idParaDest-343`

#### 4. 数据详情
`#Chapter_16.xhtml#_idParaDest-344`

#### 5. 数据指南
`#Chapter_16.xhtml#_idParaDest-345`

## 附录

### 18. 您可能喜欢的其他书籍
`#Other_Books_You_May_Enjoy.xhtml#_idParaDest-347`

### 19. 索引
`#Index.xhtml#_idParaDest-350`

# 地标

1. 封面
2. 索引

# 前言




# 精通 Tableau 2023，第四版

本版最畅销的 Tableau 指南将教您如何在 BI 领域的各种范式中利用 Tableau 的最新功能和产品。本书更新了全新主题，涵盖 Tableau Server、Prep 和 Desktop 的最新功能，以及最新的示例和针对现实挑战的解决方案，将带您从掌握 Tableau 核心概念进阶到高级功能。此外还新增了一个关于数据治理的章节。

通读本书，您将学习如何使用各种文件和数据库连接到 Tableau Desktop 和 Prep Builder，轻松执行数据准备和处理；通过实用示例执行复杂的联接、空间联接、联合和数据混合任务。您还将掌握数据加密的执行，并探索其他专家级示例，帮助您使用 Tableau Exchange 进行计算、制图和可视化设计。

后续章节将通过实操示例教您有关提升仪表板性能、连接 Tableau Server 以及理解数据可视化的所有知识。最后，您将涵盖自助式分析、时间序列分析和地理空间分析等高级用例，并将 Tableau 连接到 Python 和 R，以在 Tableau 中实现编程功能。

最后，新增了一个全新章节，涵盖一个非常热门的主题：数据治理。您将探索 Tableau Server 重要功能的介绍，这些功能将助您做好准备，以符合 GDPR 和其他数据隐私及治理法规。

读完这本 Tableau 书籍后，您将精通 Tableau 2023，并能够应对数据分析领域的常见和高级挑战。

## 适读人群

本书专为业务分析师、商业智能专业人士和数据分析师设计，旨在帮助他们精通 Tableau，以解决一系列数据科学和商业智能问题。如果您有 Tableau 使用经验，将能更快上手相关功能，但这并非先决条件。

## 本书内容涵盖

- *第 1 章：回顾基础知识* — 带您了解开始所需的基本和核心 Tableau 概念。
- *第 2 章：准备您的数据* — 这是一个以理论为导向的章节，将帮助您理解数据准备，从而更轻松地处理混乱数据。
- *第 3 章：使用 Tableau Prep Builder* — 讨论 Tableau Prep Builder，它是 Tableau Desktop 不那么“小”的提取-转换-加载 (ETL) 兄弟产品。
- *第 4 章：了解联接、混合和数据结构* — 解答是使用关系、联接还是混合来组合数据的问题。
- *第 5 章：表计算简介* — 涵盖表计算这一特殊情况，它利用数据顺序分配排名等值。
- *第 6 章：利用 OData、数据加密、大数据和 Google BigQuery* — 教您如何利用大数据解决方案和数据加密的概念。
- *第 7 章：实践详细级别计算* — 帮助您理解这一高级主题，它可以更改特定计算的数据聚合方式。
- *第 8 章：超越基础* — 向您介绍高级可视化概念以及 Tableau Exchange、扩展程序、加速器和连接器。
- *第 9 章：使用地图* — 涵盖 Tableau 内部及部分外部制图功能，使用自定义多边形、热力图和分层地图。
- *第 10 章：使用 Tableau 进行演示* — 展示使用 Tableau 进行演示的技巧和窍门。
- *第 11 章：设计仪表板和可视化最佳实践* — 带您了解不同的格式化技巧和设计规则，以最大化可视化的美感。
- *第 12 章：利用高级分析* — 将通过三个可跟随实操的高级用例（自助式、时间序列和地理空间分析）帮助您将分析技能提升到新高度。
- *第 13 章：提升性能* — 针对性能的各个方面进行探讨，旨在赋能您创建快速加载的仪表板，以获得最佳用户体验。
- *第 14 章：探索 Tableau Server 和 Tableau Cloud* — 涵盖 Tableau Server 的不同产品和功能，包括 Ask Data 和 Data Details。
- *第 15 章：集成编程语言* — 展示如何将 R 和 Python 与 Tableau 集成，以启用（近乎）无限的分析能力。
- *第 16 章：建立数据治理实践* — 向您介绍数据治理这一广泛且重要的主题，以及如何利用 Tableau Server 实现合规。

## 如何充分利用本书

具备 Tableau 基础知识将大有裨益，并且在 14 天免费试用期结束后，您将需要 Tableau 许可证。如果您希望充分利用 *第 15 章：集成编程语言*，具备 R 或 Python 的基础知识也将有所帮助。当然，您也可以跟随步骤边学边练。

其他有用的安装将在书中根据需要提及，均属于 Windows 或 Mac 上普通免费软件安装的范畴。

## 下载示例代码文件

本书的代码包也托管在 GitHub 上：[`github.com/PacktPublishing/Mastering-Tableau-2023-Fourth-Edition`](https://github.com/PacktPublishing/Mastering-Tableau-2023-Fourth-Edition)。我们还有来自丰富图书和视频目录的其他代码包，可在 [`github.com/PacktPublishing/`](https://github.com/PacktPublishing/) 获取。快去看看吧！

## 下载彩色图片

我们还提供了一个 PDF 文件，其中包含本书使用的屏幕截图/图表的彩色图片。您可以在此下载：[`packt.link/TybKH`](https://packt.link/TybKH)。

## 约定俗成

全书使用了多种文本约定。

`CodeInText`：表示文本中的代码词、数据库表名、文件夹名、文件名、文件扩展名、路径名、虚拟 URL、用户输入和 Twitter 句柄。例如：“记录性能时，Tableau 最初会在 `My Tableau Repository\Logs` 中创建一个名为 `performance_[timestamp].tab` 的文件。”

代码块设置如下：

```
SCRIPT_REAL("x <- lm(.arg1 ~ .arg2 + .arg3 + .arg4) x$fitted", SUM(Profit), COUNT(Quantity), SUM(Sales), AVG(Discount) 
)
```

**粗体**：表示新术语、重要单词或屏幕上出现的单词（例如菜单或对话框中的单词），在文本中也以此形式显示。例如：“将 **Profit_Expected** 放在 **Columns** 货架上，紧邻 **Profit**。然后点击其中任意一个并启用 **Dual Axis**。”

警告或重要提示显示如下。

提示和技巧显示如下。

## 联系我们

我们始终欢迎读者的反馈。

**一般反馈**：发送邮件至 `feedback@packtpub.com`，并在邮件主题中注明书名。如果您对本书的任何方面有疑问，请发送邮件至 `questions@packtpub.com`。

**勘误**：虽然我们已尽一切努力确保内容的准确性，但错误难免发生。如果您在本书中发现错误，我们将不胜感激如果您能向我们报告。请访问 [`www.packtpub.com/submit-errata`](http://www.packtpub.com/submit-errata)，选择您的书籍，点击 **Submit Errata**，并填写表格。

**盗版**：如果您在互联网上以任何形式发现我们作品的非法副本，我们将不胜感激如果您能向我们提供该位置地址或网站名称。请通过 `copyright@packtpub.com` 联系我们，并附上相关材料链接。

**如果您有兴趣成为作者**：如果您在某个主题上有专长，并有兴趣撰写或为书籍做贡献，请访问 [`authors.packtpub.com`](http://authors.packtpub.com)。

## 分享您的想法




读完 *Mastering Tableau 2023 - Fourth Edition*（《精通 Tableau 2023 - 第四版》）后，我们非常期待听到您的想法！请[点击此处直接前往亚马逊评论页面](https://www.packtpub.com/)为本书留下您的反馈。

您的评论对我们和技术社区都至关重要，这将帮助我们确保交付高质量的内容。

# 下载本书的免费 PDF 版本

感谢您购买本书！

您是否喜欢在途中阅读，但无法随身携带纸质书？

您购买的电子书是否与您选择的设备不兼容？

别担心，现在购买每一本 Packt 图书，您都可以免费获得该书的无 DRM 限制的 PDF 版本。

随时随地，在任何设备上阅读。直接从您喜爱的技术书籍中搜索、复制并粘贴代码到您的应用程序中。

福利不止于此，您还可以每天在收件箱中获得独家折扣、新闻通讯和优质免费内容。

请遵循以下简单步骤以获取权益：

1.  扫描二维码或访问下方链接

![](img/B18435_Free_PDF_QR.png)

[`https://packt.link/free-ebook/9781803233765`](https://packt.link/free-ebook/9781803233765)

2.  提交您的购买凭证

3.  就这些！我们会将免费 PDF 和其他权益直接发送到您的邮箱

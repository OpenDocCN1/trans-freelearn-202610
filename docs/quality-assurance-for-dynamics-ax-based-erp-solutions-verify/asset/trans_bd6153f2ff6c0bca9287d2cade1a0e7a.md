
![](img/9ad0bc1af82319325b21eed41dd3b28e_0_0.png)

从技术到解决方案

## 基于 Dynamics AX 的 ERP 解决方案的质量保证

验证 Dynamics AX 定制是否符合 Microsoft IBI 标准

Anil Kumar Gupta

PACKT PUBLISHING

## 基于 Dynamics AX 的 ERP 解决方案的质量保证

验证 Dynamics AX 定制是否符合 Microsoft IBI 标准

Anil Kumar Gupta

## 基于 Dynamics AX 的 ERP 解决方案的质量保证

版权所有 © 2008 Packt Publishing

保留所有权利。本书的任何部分，未经出版者事先书面许可，不得以任何形式或任何手段复制、存储于检索系统或传播，但用于评论文章或评论中简短引文的情况除外。

在编写本书时已尽最大努力确保所提供信息的准确性。然而，本书所含信息按“现状”提供，不提供任何明示或暗示的保证。作者、Packt Publishing 及其经销商或分销商不对因本书直接或间接造成或声称造成的任何损害承担责任。

Packt Publishing 已努力通过适当使用大写字母为本书中提及的所有公司和产品提供商标信息。然而，Packt Publishing 不能保证该信息的准确性。

首次出版：2008年2月

生产参考：1280208

由 Packt Publishing Ltd. 出版
32 Lincoln Road
Olton
Birmingham, B27 6PA, UK.

ISBN 978-1-847192-91-2

www.packtpub.com

封面图片由 Vinayak Chittar 提供 (vinayak.chittar@gmail.com)

## 鸣谢

|  |  |
|---|---|
| **作者** Anil Kumar Gupta | **项目经理** Abhijeet Deobhakta |
| **审阅者** David Bowles | **项目协调员** Abhijeet Deobhakta |
| **策划编辑** Priyanka Baruah | **索引** Monica Ajmera |
| **开发编辑** Ved Prakash Jha | **校对** Chris Smith |
| **技术编辑** Akshara Aware | **制作协调员** Shantanu Zagade |
| **编辑团队负责人** Mithil Kulkarni | **封面设计** Shantanu Zagade |

## 关于作者

Anil Kumar Gupta 是一位经验丰富的资讯科技专业人士，在软件测试管理及软件流程改进方面拥有 proven 经验。他曾领导多个多样化软件项目中的各种计划，如 ERP、电子政务、电子银行、电子学习、系统软件、应用软件等，并结合商业现成产品 COTS、业务流程管理 BPM、客户关系管理 CRM 和企业资源规划 ERP 技术解决方案、J2EE 和 .Net，以满足并超越商业和政府部门的挑战性业务需求。他的 10 多年专业职业生涯涵盖了众多领域，包括但不限于系统工程与集成、金融服务，以及使用计算机工具解决复杂工程问题的制造业和流程工业 ERP。凭借出色的分析能力和沟通技巧，以及与管理层、供应商、客户和员工的有效互动，他能够在压力下按时完成具有挑战性的期限。同时，Mr. Gupta 识别、管理并解决了多个复杂的项目任务，其中包括在团队和单人环境中不断变化的优先事项。Mr. Gupta 提供强有力的协作管理，任务导向并注重所需细节和准确性。Mr. Gupta 是一位自我激励、精力充沛、可靠、灵活、果断、细心且富有资源的管理者，提供有效的团队支持，结合人际、协调、指导以及口头和书面沟通技巧。

在他职业生涯中，他遇到了许多挑战，其中包括在 EuroInfo Systems Pvt. Ltd. 建立 QA 和测试实践。EuroInfo Systems Pvt. Ltd. 目前被称为 Tectura India。Tectura India 是印度最大的 MBS 解决方案提供商，全球范围内 Tectura 也是最大的 MBS 解决方案提供商。EIS 于 2004 年率先设立独立的 QC 部门，这一挑战交给了 Anil。由于他成功的质量举措，EIS 在 1000 家 MBS 合作伙伴中获得了 2005 年度的全球 Axapta 卓越奖。作者是印度 Dynamics AX 测试方面最有经验的资源，并为多个符合 IBI 标准的 Dynamics AX ISV 项目工作过。

我感谢您购买本书。希望您喜欢其内容、信息流以及在实际工作中提供的帮助。

我为使本书成功付出了努力，我并不是唯一将本书带到您手中的人。我想感谢帮助过我的人。

首先，我要感谢 Lord Sai，他启发我撰写本书，并在写作和出版过程中一直帮助我。其次，我要感谢我的父母，他们的祝福始终伴随着我，这不仅给了我能量，也在各方面帮助了我。

我相信家庭在成功中发挥着重要作用。我的妻子一直激励我并安排好一切，使我能将最大时间投入到本书中。我惊讶地发现，我 4 岁的孩子理解了我的局限，在我工作时从不打扰我。

我想感谢 Packt Publishing 认为这个主题有趣。我特别要感谢 Mr. Ved Prakash Jha（开发编辑）、Ms. Akshara Aware（技术编辑）和 Priyanka Baruah，他们辛勤工作并在各方面与我合作，使本书取得成功。

最后，我要感谢所有名字未在此列出但对本书成功做出贡献的人。

## 关于审阅者

**David Bowles** 在他二十八年的职业生涯中，自四岁起就与计算机打交道，并在支持、系统管理、开发和游戏方面参与计算机。虽然他在大学早期并不特别喜欢编程和软件开发，但他在五岁时在 VIC-20 上用 BASIC 编程，并得到 BYTE 杂志的一些帮助，这预示了他日后在软件开发方面的道路。虽然他成长于佐治亚州南部 Albany 和 Moultrie 的小镇，但他有幸通过父亲的雇主接触到计算机，并在后来有能力负担得起。

David 在加入 NAPA Rayloc 工作后不久就接触到 Dynamics AX。执行层决定从基于旧式 COBOL 的会计系统迁移到 Dynamics AX。David 获得了该软件的 Virtual PC 镜像，并被给予几周时间熟悉它。在试用基础上工作和开发软件三个月后，实施过程开始。在为期十二个月的实施期间，他是项目中唯一的全职开发人员，经历了许多漫长的白天黑夜的编码、一场婚礼以及几次 48 小时以上的加班，Rayloc 完成了 AX 的全面实施。在实施和学习过程中，David 获得了 Microsoft 最有价值专业人士奖 Dynamics AX。获奖主要基于他在论坛、博客中对用户社区的贡献，以及他对新加入的 4.0 版应用程序集成框架的研究反馈给 Microsoft 支持。在加入 NAPA Rayloc 之前，David 在 AOL、SurfSouth、CallTech Communications 和 Kodak 担任技术和编程职位。

目前 David 已与他理想的妻子 Courtney 结婚，他们居住在佐治亚州亚特兰大西部郊区。除了工作之外，他还在亚特兰大的 Mercer University 就读，以完成工商管理理学学士学位。当他不在工作或学习时，他喜欢在高尔夫、网球、彩弹射击以及偶尔的 XBox 游戏中度过业余时间。


# 从人类反馈中强化学习实用指南：利用人类信号对齐 AI 模型

## 欢迎来到 Packt 抢先体验

**欢迎来到 Packt 抢先体验**。我们在本书正式发售前为您提供独家预览。写一本书可能需要数月时间，但我们的作者拥有前沿信息，可与您即时分享。抢先体验通过提供章节草稿，让您洞悉最新进展。章节目前可能略显粗糙，但作者会随时间更新。您可以浏览本书的任意章节，也可以从头到尾跟读；抢先体验旨在灵活多变。希望您享受了解 Packt 图书写作过程的乐趣。

## 目录

- [输出质量指标](`text/ch012.xhtml#output-quality-metrics`)
- [任务特定与对齐指标](`text/ch012.xhtml#task-specific-and-alignment-metrics`)
- [安全性与鲁棒性指标](`text/ch012.xhtml#safety-and-robustness-metrics`)
- [偏好代理评估](`text/ch012.xhtml#preference-proxy-evaluations`)
- [人工评估](`text/ch012.xhtml#human-evaluation`)
- [LLM 作为评判者](`text/ch012.xhtml#llm-as-a-judge`)
- [总结](`text/ch012.xhtml#summary-10`)
- [参考文献](`text/ch012.xhtml#references-10`)
- [第 12 章：其他应用](`text/ch013.xhtml#beyond-language-aligning-ai-across-modalities`)
  - [加入我们的 Discord 图书社区](`text/ch013.xhtml#join-our-book-community-on-discord-11`)
  - [技术要求](`text/ch013.xhtml#technical-requirements-10`)
  - [从文字到像素：图像与视觉生成](`text/ch013.xhtml#from-words-to-pixels-image-and-visual-generation`)
  - [教会机器移动](`text/ch013.xhtml#teaching-machines-to-move`)
  - [谐和声音：音频与音乐生成](`text/ch013.xhtml#harmonizing-sound-audio-and-music-generation`)
  - [总结](`text/ch013.xhtml#summary-11`)
  - [参考文献](`text/ch013.xhtml#references-11`)
- [封面](`text/cover.xhtml`)

## 第 1 章：强化学习简介

### 加入我们的 Discord 图书社区

![中间带方块的二维码，描述自动生成](`media/file0.png`)

[https://packt.link/EarlyAccessCommunity](https://packt.link/EarlyAccessCommunity)

本书旨在探讨如何使 AI 代理与人类意图保持一致。过去二十年间，AI 能力呈指数级增长，机器学习、计算机视觉、自然语言处理、深度学习和**强化学习**(`RL`)等学科的里程碑式进展在其中发挥了关键作用。这些能力和应用展示了 AI 扩展得有多好，以及它如何影响几乎所有事物。这使得 AI 的行为或采取的行动与人类意图保持一致变得至关重要，而这一必要性也是安全使用该技术的途径。在本书中，我们将深入探讨**基于人类反馈的强化学习**(`RLHF`)，它在生成式预训练模型的成功中发挥了关键作用，并将其推向主流技术应用。本书第一部分从总体角度介绍 `RLHF`，并帮助读者理解将 `RLHF` 应用于任何应用所需的核心技术组件。第二部分深入探讨将 `RLHF` 应用于语言建模，特别是大型语言建模。最后一部分为读者提供该技术的当前趋势和未来展望。在本章中，我们首先涵盖 `RL` 的基础知识和理解 `RL` 算法的关键概念，通过高层次解释其类别，向读者介绍 `RL`。然后我们介绍一个称为网格世界的特定简单经典环境，并展示如何实现 `Q-learning` 这一基本 `RL` 算法。利用基本实现进行比较和提供背景，我们随后详细说明如何设计奖励函数，以及如何实现迁移学习以加速 `RL` 训练。最后，我们展示**人类反馈**如何辅助基于 `RL` 的训练，并在同一示例上进行动手实现，以便读者能清晰地比较其相对于经典 `Q-learning` 实现的改进。这让读者了解 `RLHF` 在实践中如何运作，并为后续章节深入探讨更复杂的算法和架构奠定基础。本章将涵盖以下主要标题：

- 强化学习要点
- 理解 RL 算法
- 探索 RL 环境：网格世界环境
- 设计奖励函数与迁移学习
- 基于人类反馈的强化学习

### 技术要求

本章将所有实现相关的依赖保持最小化。所有实现均使用 `python`。具体版本建议和所需安装的最低库如下：

- `Python` 版本 >= 3.9
- `Numpy` == 1.26.4
- `Matplotlib` == 3.8.4

开始前，请克隆此 Git 仓库：[`https://github.com/PacktPublishing/A-Practical-Guide-to-Reinforcement-Learning-from-Human-Feedback`](https://github.com/PacktPublishing/A-Practical-Guide-to-Reinforcement-Learning-from-Human-Feedback)，并将目录切换到 `chapter1`。

### 强化学习要点


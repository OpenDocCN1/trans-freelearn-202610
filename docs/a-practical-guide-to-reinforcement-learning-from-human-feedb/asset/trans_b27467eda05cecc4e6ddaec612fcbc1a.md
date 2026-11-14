

## 参考资料

### TRL 文档
- [https://huggingface.co/docs/trl/reward_trainer](https://huggingface.co/docs/trl/reward_trainer)

### 论文
- [https://openreview.net/pdf?id=MnfHxPP5gs](https://openreview.net/pdf?id=MnfHxPP5gs)
- [https://direct.mit.edu/coli/article/34/4/555/1999/Inter-Coder-Agreement-for-Computational](https://direct.mit.edu/coli/article/34/4/555/1999/Inter-Coder-Agreement-for-Computational)
- [https://aclanthology.org/J08-4004.pdf](https://aclanthology.org/J08-4004.pdf)
- [https://arxiv.org/pdf/2312.09244](https://arxiv.org/pdf/2312.09244)
- [https://openreview.net/pdf?id=gkfUvn0fLU](https://openreview.net/pdf?id=gkfUvn0fLU)
- [https://aclanthology.org/2024.emnlp-main.174.pdf](https://aclanthology.org/2024.emnlp-main.174.pdf)

# 第 8 章：用于调优语言模型的强化学习

## 加入我们的图书社区

![一个中间带方块的二维码，描述自动生成](media/file0.png)

[https://packt.link/EarlyAccessCommunity](https://packt.link/EarlyAccessCommunity)

本章的目标是提供对强化学习如何利用基于人类偏好数据训练的奖励模型来微调预训练语言模型的实践性和概念性理解。该阶段通过将偏好信号转化为优化目标，完成了基于人类反馈的强化学习（`RLHF`）流水线，使得能够使用稳定的强化学习技术（如近端策略优化 `PPO`）实现可衡量且对齐的模型行为。您将学习如何将训练好的奖励模型集成到强化学习循环中，利用该奖励信号微调策略模型，并使用 Transformer Reinforcement Learning（`TRL`）库等工具实现此流水线。本章直接建立在监督微调（第 6 章）和奖励建模（第 7 章）的基础之上，重点在于闭环并使生成行为与人类期望对齐。`RLHF` 的这一最终阶段至关重要，因为它将隐性的人类反馈转化为显性的优化。没有它，模型仅部分对齐；有了它，模型才能响应现实部署中重要的价值观、目标和安全约束。掌握这一步对于构建不仅强大，而且值得信赖且对齐的语言模型至关重要。

## 关键主题

- 奖励模型与强化学习的集成
- 用于 `RLHF` 的 `PPO` 算法
- 使用 Transformer Reinforcement Learning（`TRL`）库中的 `PPO` Trainer 进行微调
- `PPO` 微调中需跟踪的指标
- 评估微调后的模型

## 技术要求

本章我们将使用 `Python`，并将所有实现相关的依赖保持最小化。具体的版本建议和所需安装的最小库如下：

1. `Python` 版本 >= 3.9
2. `numpy` == 1.26.4
3. `matplotlib` == 3.8.4
4. `transformers` == 4.42.4
5. `datasets` == 3.0.1
6. `ipython` == 8.27.0
7. `ipywidgets` == 8.1.5
8. `trl` == 0.10.1

除了这些库和安装要求外，我们将使用 `Google Colab` 利用 `GPU` 微调预训练语言模型。要开始，请克隆此 git 仓库并切换目录到 `chapter 8`：

```
git clone https://github.com/PacktPublishing/A-Practical-Guide-to-Reinforcement-Learning-from-Human-Feedback
cd chapter 8
```

## 奖励模型与强化学习的集成

奖励模型（第 7 章详细讨论）可用于模拟人类在面对示例选择时如何评分或给出偏好。训练好的奖励模型可用于改进策略优化，并使语言模型与人类偏好对齐。奖励模型如何与 `RLHF` 的其他组件连接的高级示意图如 *图 8.1* 所示。奖励模型针对给定的用户提示，输出生成的模型响应的信号（奖励）或分数。在训练期间，它利用人工标注的分数或偏好来比较其预测以改进模型。一旦训练完成，模型会为输入文本输出奖励或分数。该奖励用于计算策略模型如何更新。

![图 8.1 RLHF 中作为优化策略模型一部分的奖励模型](media/file36.png)

*图 8.1 RLHF 中作为优化策略模型一部分的奖励模型*

在实际的 `RLHF` 设置中，提供给奖励模型作为输入的状态-动作对通常是 **用户提示** 及其 **模型生成的响应**。用户提示可以是问题、聊天消息、图像或任何其他形式的数据，模型可能被要求生成文本响应，可以是聊天消息或回复、图像、代码，或简单的问答对。奖励模型的输出（奖励）用于更新策略模型权重，更新过程基于强化学习。奖励模型已经在大量相关的用户提示和带有人类评分的示例响应样本上训练完成。

与标准 RL 中奖励函数基于观察计算奖励或观察的奖励分配显式映射不同，`RLHF` 依赖于奖励模型的 **预测**。该模型通常基于神经网络或偏好模型（如 **Bradley-Terry 模型**）。这意味着预测奖励是基于人类反馈对状态-动作对的期望程度或质量的估计，而不是显式的奖励分配。在标准 RL 中，计算出的奖励决定了策略模型权重需要更新多少。在这种情况下，我们改用奖励模型推理来更新策略模型。对于涉及自然语言等应用，显式奖励分配不可行，奖励模型是一种实用的替代方案。

奖励模型基于使用人工标注的状态-动作对进行训练，这些数据可能不一致、主观、嘈杂且依赖于上下文。将奖励推理泛化到新场景是一种要求，在实践中也常被期望。幸运的是，学习到的奖励中的不确定性和泛化能力可以用类似处理任何监督学习模型的方式来处理。

来自奖励模型的预测奖励通常会被归一化，以确保它们在工作范围内。这通常基于实验，以便最小化策略优化中的不稳定性。奖励模型的输出（奖励信号）可用于训练策略模型。策略优化可以尝试任何 RL 算法；然而，由于在线算法（特别是 `PPO`）训练相对稳定，因此更受青睐。在下一节中，我们将更多地讨论 `PPO` 算法。

## 用于 RLHF 的 PPO 算法

我们在第 4 章深入介绍了 `PPO` 算法。在本节中，我们将从 `RLHF` 标准算法选择的角度进行讨论，因为 `PPO` 在训练稳定性和样本效率方面提供了良好的平衡。在在线算法中，它具有更简单的裁剪机制实现，该机制可减弱来自前一次迭代的策略更新，从而提高稳定性。虽然离线算法可以具有样本效率，但 `PPO` 通过重用收集的数据进行多次更新来提高样本效率，使其比其他策略梯度算法更具样本效率。由于其在稳定性和效率之间的平衡，以及在各种环境中经过验证的有效性，它是包括 `RLHF` 在内的 RL 应用中的默认选择。其广泛采用也归因于其在训练中的相对稳定性和可靠性，这对于管理具有不确定性的大型系统训练至关重要。

### 使用奖励模型的 PPO 策略更新


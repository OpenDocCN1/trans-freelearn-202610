## 第 3 章：奖励建模

![中间带有一个正方形的二维码 描述自动生成](img/file0.png`)

<https://packt.link/EarlyAccessCommunity>

在上一章中，我们讨论了 RLHF 的作用以及将人类反馈融入以对齐 AI 策略并加速训练的各种方法。一种实用的方法是使用人类反馈训练奖励模型，该模型最终可以指导智能体训练。在本章中，我们将重点关注奖励建模过程。关键概念是将智能体必须实现的**目标规范过程**与**如何实现该目标的过程**分离开来。我们将在本章涵盖以下主要主题：

*   强化学习中奖励的重要性
*   奖励规范中的挑战
*   奖励建模技术
*   在奖励设计中融入人类反馈

在本章结束时，读者将获得如何开发和训练奖励模型、将其与强化学习算法集成以训练智能体的详细实操理解。当奖励规范具有挑战性且可以使用人类反馈时，这将为他们解决强化学习问题提供一个新工具。

### 技术要求

对于本章，我们将所有与实现相关的依赖项保持在最低限度。所有实现均使用 `Python`。具体版本建议和所需安装的最低库如下：

*   `Python` 版本 `>= 3.9`
*   `Numpy` `== 1.26.4`
*   `Matplotlib` `== 3.8.4`
*   `pytorch` `== 2.3.0`





开始之前，请克隆此 Git 仓库：[`github.com/PacktPublishing/A-Practical-Guide-to-Reinforcement-Learning-from-Human-Feedback`](https://github.com/PacktPublishing/A-Practical-Guide-to-Reinforcement-Learning-from-Human-Feedback)，并将目录切换到 `chapter3`。

# 强化学习中奖励的重要性

在前几章中，我们介绍了强化学习（RL）和**状态-动作-奖励框架**，智能体通过与环境交互来学习最优决策策略。现在我们将更详细地关注奖励本身。智能体选择动作的决策基于优化一个目标，而奖励的设计旨在达到或维持该目标。当智能体最大化奖励时，反过来也提高了其达成预期目标的几率。智能体的训练效率以及最终训练出的智能体的性能，取决于奖励设计得有多好。奖励设计根据智能体采取的动作，为其提供关于表现好坏的反馈机制，从而驱动学习过程。这种反馈通过激励智能体探索环境并调整行为以符合预期的人类目标，来驱动学习过程。通过奖励分配明确阐述期望行为，奖励设计帮助智能体更高效地达成目标，并提升其整体性能。此外，对采取动作的清晰反馈能加速智能体的学习过程。一个定义良好的奖励结构使智能体能更有效地适应环境。例如，在迷宫导航任务中，如果奖励设计能适当引导机器人的动作，它更有可能成功到达目标并避开障碍物。

## 外部奖励与内部奖励

**外部奖励** 是当智能体受到其交互环境影响时分配的，例如到达目标状态或撞上障碍物。相反，**内部奖励** 与环境效应无关，而是以特定方式行事的激励。考虑*第 1 章*中的网格世界示例，到达目标状态获得 *+10* 奖励，撞上障碍物获得 *-5* 奖励，两者均基于环境交互，属于外部奖励。同样，通过分配 *-1* 奖励来惩罚迈出一步，向智能体发出信号以用最少步数到达目标，因为该奖励直接关联到高效完成任务这一环境实体，所以它是一个塑形的外部奖励。反之，可以通过鼓励探索和好奇心来提供行为激励，例如访问新单元格奖励 *+0.5*，这是内部奖励，因为它仅激励智能体行为，独立于环境。

## 奖励塑形及其如何加速训练

**奖励塑形** 是修改奖励函数以提供额外的中间奖励，从而引导智能体进行更高效学习的过程。提供**中间奖励**是**奖励塑形**的一种技术，我们在任务中为达成子目标或里程碑引入奖励，以此标示向达到最优**策略**迈进的进度。奖励塑形的一种互补技术是分配方向性奖励。方向性奖励是基于智能体朝主目标的进度或移动给予的奖励。**中间奖励**和**方向性奖励**都能加速智能体训练。虽然**中间奖励**侧重于为达成子目标提供结构化反馈，但**方向性奖励**则基于智能体向主目标的进度提供持续反馈。同时使用这两种技术，可以通过在整个学习过程中提供清晰且频繁的指导，帮助智能体更有效地学习。在网格世界中，如果主目标是到达右下角，到达特定的中间单元格或检查点可能会获得**中间奖励**。这些中间单元格可能并非离主目标最近，但有助于避开障碍物。相比之下，**方向性奖励**可能会因靠近目标而给予（例如每靠近一步 *+0.5*），因远离目标而惩罚（例如每远离一步 *-0.15*）。虽然中间奖励和方向性奖励侧重于性能和达成目标（即知识的利用），但对于包含未知区域的复杂环境，探索环境也是合理的。为了鼓励探索，奖励智能体探索新状态或动作以促进全面学习是一种明智的策略。这可以通过分配**探索奖励**给智能体来实现。在用于训练智能体的奖励信号背景下，奖励规范或设计直接影响智能体的训练性能。虽然奖励塑形可以加速训练，但设计奖励函数仍需要一定程度的审慎和细致。奖励信号需要具有信息量。在此语境下，**信息量** 指奖励信号为智能体提供了多少关于如何改进其行为的有用信息（定义见《面向强化学习智能体的可解释奖励设计》：https://proceedings.neurips.cc/paper/2021/file/a7f0d2b95c60161b3f3c82f764b1d1c9-Paper.pdf )[1]。信息量更大的奖励能给智能体更清晰的指导，指明哪些动作有益、哪些无益。一个相关概念是**稀疏性**，指奖励给予的频率。稀疏奖励给予频率低，通常仅在重大成就时给予；而密集奖励则为较小的成就更频繁地给予。如果奖励过于稀疏，智能体可能难以学习策略，因为它接收到的反馈不足以指导行为。如果奖励过于密集但缺乏信息量，智能体可能学到次优行为，或花费更长时间理解最有效的策略。因此，需要一种平衡的方法。通过平衡**信息量**和**稀疏性**，奖励结构可以提供频繁且有意义的反馈，帮助智能体高效有效地学习。虽然实现这种平衡有相关实践，但它们非常依赖具体情况，并开始需要相当的技巧和领域知识。接下来我们将更详细地讨论指定奖励时面临的挑战。

# 奖励规范中的挑战

奖励规范与设计对智能体训练至关重要，但面临诸多挑战。本节将讨论其中一些，并了解可能有助于克服它们的替代方案。

## 目标错位

智能体学到的目标与人类意图之间的错位，可能源于模棱两可的奖励。模棱两可的奖励可能导致非预期行为。很难通过奖励精确指定期望的行为和结果。另一个因素可能是任务复杂性本身。在涉及多个目标的复杂任务中，用单一奖励函数准确捕捉目标极具挑战性。

## 稀疏与延迟奖励

在许多环境中，奖励是稀疏且延迟的，或两者兼具。稀疏奖励因对动作成功与鲜少反馈而导致探索挑战。虽然奖励塑形技术可以缓解部分稀疏性问题，并如《面向强化学习智能体的可解释奖励设计》（https://proceedings.neurips.cc/paper/2021/file/a7f0d2b95c60161b3f3c82f764b1d1c9-Paper.pdf）中详述的那样呈现近视最优学习 [1]，但延迟奖励会造成对当前动作成功与否及奖励的困惑。

## 奖励黑客




# 奖励黑客攻击与奖励设计中的挑战

智能体可能会通过利用奖励函数中的漏洞来获取高奖励，而不是表现出期望的行为。这被称为奖励黑客攻击。设计不当的奖励可能导致最大化奖励但却是不可取的、不安全的，或以非预期方式与目标不一致的行为。考虑一个清洁机器人的例子，它有一个代理奖励函数，鼓励清洁尽可能多的区域。机器人可能会打翻一个珍贵的花瓶以最大化其清洁得分，尽管打碎花瓶是不可取的。在这种情况下，代理奖励未能考虑到这些非预期后果。

## 设计复杂性

设计奖励函数通常需要领域知识和专业知识，这对人类来说既耗时又容易出错。此外，在动态或变化的环境中，固定的奖励函数可能不合适，这需要持续调整。

## 多目标的权衡与平衡

在单一奖励函数中平衡多个目标可能具有挑战性。例如，智能体可能需要平衡速度和安全，这两者可能是相互冲突的目标。在网格世界中，如果障碍物位于通往目标状态的最短路径上，智能体必须先绕过障碍物。就当前步骤而言，避开障碍物的目标可能与到达目标的目标相冲突。虽然这可以通过设计任务层级和加权奖励函数来解决，但在复杂环境中，确定奖励函数不同组成部分的适当权重可能很困难，并可能需要大量调整。

## 可扩展性

对于具有多个目标的大型环境，单一奖励函数可能无法很好地扩展。在这种情况下，创建任务层级可能比较合适。对于具有层级结构的任务，设计能在层级的不同层面适当激励进展的奖励本身就是一项复杂的任务。

## 克服挑战的策略

为了克服上述挑战，可以采用几种策略。虽然并非所有策略在所有场景下都完全有效，但它们可以作为值得尝试的有前景的方法。例如，可以通过将任务分解为具有特定目标的更小、更简单的子任务来解决任务的复杂性。这被称为**任务分解**。例如，一个拾取并放置物体的拾取放置机器人任务可以分解为三个更简单的任务：1) 在不碰到障碍物的情况下到达目标物体，2) 抓取物体，3) 将物体移动到目标位置并释放。奖励定义的其他问题可以通过根据智能体的表现和行为迭代设计、测试和改进奖励函数来识别和改进。使用模拟和受控环境，可以观察智能体的行为并相应地调整奖励信号。通过实现和比较奖励函数的多个版本，我们可以进行 A/B 测试，以检验它们在实现期望行为方面的有效性。通过观察人类专家的行为，利用逆向强化学习推断奖励函数，也有助于捕捉专家执行任务时使用的隐性目标。推断奖励函数的一个例子是观察拾取放置用例中专家机器人运动的状态-动作轨迹，其中推断分配给多个分解目标的权重，如避障、到达目标的效率（耗时）或安全性和合规性（如速度或加速度），可以作为迈向 IRL 的步骤。虽然所有这些策略在某些场景下都可能有效，但它们需要人类的专业知识和实验。更容易融入人类反馈的一种方法是在学习过程中添加人类反馈，以指导智能体并纠正不一致的行为。这可以通过使用人类偏好并将其纳入奖励建模过程来实现。

# 奖励建模技术

奖励建模技术多种多样，可以根据反馈来源、涉及的学习类型以及将人类偏好整合到奖励函数中的具体方法进行广泛分类。有几种人类反馈来源，取决于如何感知或接收反馈。以下列举了一些来源。

## 人类反馈来源

### 专家反馈

人类专家提供期望的结果或引导智能体朝向特定目标。这对于直接定义奖励函数可能具有挑战性的复杂任务很有帮助。

### 环境反馈或试错法

智能体通过与环境的交互进行学习。成功带来正奖励，而失败带来负奖励。这是一种更具探索性的方法，但对于简单环境可能有效。智能体通过与环境本身交互来学习。环境反馈是智能体因其动作而从环境接收到的原始、未经处理的信息。这可以是：

*   **成功/失败信号：** 智能体是否达成了目标？
*   **状态变化：** 环境因动作而如何改变？
*   **资源变化：** 智能体是获得还是失去了资源？

原始信息为奖励模型提供背景，例如原始信号可以作为数据点，以学习智能体动作、环境结果状态与**期望结果**之间的关系。例如：

*   在一个收集金币的游戏中，原始信息可能告诉模型智能体撞墙了（状态变化）。这本身并不能告诉模型这是好结果还是坏结果。
*   然而，通过将此信息与其他数据点（如未收集到金币）相结合，模型可以学到撞墙通常是不好的（负奖励）。

在 RLHF 背景下，人类反馈可用于融入环境信号或增加其清晰度。

### 组合反馈

这种方法利用人类和环境反馈来指导学习过程。

## 基于偏好的学习

该技术利用人类对不同轨迹或动作的偏好来学习奖励函数。该过程包含以下三个步骤：

1.  **数据收集**：收集轨迹或动作对，并请人类提供它们之间的偏好。
2.  **偏好建模**：使用模型（通常是神经网络）学习一个预测人类偏好的函数。
3.  **奖励函数学习**：模型通过优化人类提供的偏好的似然性，学习一个与人类偏好一致的奖励函数。

假设我们有两条轨迹 和 。偏好 表示轨迹 优于 。模型被训练以最大化预测奖励 大于 的概率。更详细的讨论请参阅《基于偏好的强化学习方法综述》：[`www.jmlr.org/papers/volume18/16-634/16-634.pdf`](https://www.jmlr.org/papers/volume18/16-634/16-634.pdf)，《来自人类偏好的深度强化学习》：[`arxiv.org/abs/1706.03741`](https://arxiv.org/abs/1706.03741)，以及《不完全区组设计的秩分析：I. JSTOR 上的成对比较法》：[`www.jstor.org/stable/2334029?origin=crossref`](https://www.jstor.org/stable/2334029?origin=crossref)。

## 绝对评分模型

这些模型直接为单个输出估算分数，反映其与人类偏好的契合程度。这可以被构建为一个回归任务。与依赖相对比较的基于偏好的学习不同，绝对评分模型使用直接的数值分数。

## 多目标奖励模型



在现实场景中，人类偏好可能涉及多个方面。这些模型学习在同时考虑这些目标的情况下分配奖励（例如，`Absolute-Rating Multi-Objective Reward Model` (`ArmoRM`)，正如 `Interpretable Preferences via Multi-Objective Reward Modeling and Mixture-of-Experts` 中所讨论的：[`arxiv.org/abs/2406.12845`](https://arxiv.org/abs/2406.12845)）。

## 逆强化学习 (IRL)

`Inverse Reinforcement Learning` (`IRL`) 旨在从观察到的专家行为中推断潜在的奖励函数。`IRL` 基于这样一个前提：专家行为相对于某个未知的奖励函数是最优的。通过观察专家采取的动作，`IRL` 试图反向工程专家隐式优化的奖励函数。这涉及两个主要步骤：

1.  **专家演示**：收集专家执行任务时的轨迹或状态和动作序列。这些轨迹代表了专家的行为，并作为 `IRL` 的主要数据。
2.  **奖励函数估计**：使用优化技术推断奖励函数，使观察到的专家行为显得最优。这涉及将问题表述为一个优化任务，目标是找到最能解释专家动作的奖励函数。

所有奖励建模方法的区别在于收集何种人类信号以及如何利用它们来预测奖励。它们都涉及一个通用的监督学习步骤，即使用输入数据训练模型以学习预测输出（即奖励）。它可用于学习难以手动定义的复杂奖励函数，特别是在大型或动态环境中。模型可以通过额外数据进行潜在微调，以适应略微修改的环境或目标，使其更具适应性和可扩展性。读者可参考 `Maximum Entropy Inverse Reinforcement Learning`：[`cdn.aaai.org/AAAI/2008/AAAI08-227.pdf`](https://cdn.aaai.org/AAAI/2008/AAAI08-227.pdf) 以获取更多信息。

## 奖励模型类

仓库中 `reward_models.py` 文件里的 `RewardModel` 类，定义了用于预测奖励的基于神经网络的模型架构。它继承自 `nn.Module`，并定义了一个将被训练的前馈神经网络。`nn.Module` 是 PyTorch 中的一个基础类，PyTorch 是一个流行的深度学习库，用于构建神经网络。继承自 `nn.Module` 使 `RewardModel` 能够访问构建神经网络层的功能。此函数在创建期间初始化模型的参数。它接受三个参数：`state_size`：智能体状态表示的维度（例如机器人的位置）。这里设置为 2（可根据您的具体问题进行调整）。`hidden_size`：隐藏层中的神经元数量，控制模型的复杂度（此处设置为 64）。`output_size`：模型输出的奖励值数量。在本例中，设置为 3，可能对应多个奖励类别（例如进度、安全性和目标效率）。模型由两个全连接层（`fc1` 和 `fc2`）组成。`fc1`：接收拼接后的状态和下一状态（组合大小为 `state_size * 2`）作为输入，并通过 `ReLU` 激活函数。该层从组合的状态信息中提取相关特征。`fc2`：接收第一层的输出并将其转换为所需数量的奖励（`output_size`）。

```python
# RewardModel class definition
class RewardModel(nn.Module):
    def __init__(self, state_size=2, hidden_size=64, output_size=3):
        super(RewardModel, self).__init__()
        self.fc1 = nn.Linear(state_size * 2, hidden_size)  # Adjusted to take both state and next_state as input
        self.fc2 = nn.Linear(hidden_size, output_size)  # Output 3 rewards: progress, safety, and goal-efficiency
    def forward(self, state, next_state):
        x = torch.cat((state, next_state), dim=1)  # Concatenate state and next_state
        x = torch.relu(self.fc1(x))
        rewards = self.fc2(x)
        return rewards
```

`forward` 函数定义了数据如何流经网络。它接受当前状态和下一状态作为输入参数。它将状态和下一状态拼接，为网络创建单一输入。然后将组合的状态信息通过带有 `ReLU` 激活的第一层。最后，将第一层的输出送入第二层，从而得到预测的奖励。通过使用人类反馈数据训练此 `RewardModel`，模型学会将特定的状态转换与符合人类偏好的奖励联系起来。然后利用这些信息引导 RL 智能体朝向人类认为可取的行为。这只是 RLHF 集成的第一步。在本章后续章节中，将探讨如何使用人类反馈数据训练此奖励模型，以及如何将其纳入 RL 智能体的学习过程中。

## 在奖励设计中融入人类反馈

在讨论的奖励建模技术中，基于偏好的学习、绝对评分模型和多目标奖励模型适合直接整合人类信号反馈。直接整合人类反馈通常优于仅依赖 `Inverse Reinforcement Learning` (`IRL`)，因为后者在收集人类反馈方面存在局限性：

*   **间接性：** `IRL` 基于观察推断奖励函数。这容易受到观察行为中错误和偏见的影响。
*   **复杂性：** `IRL` 算法在计算上可能非常昂贵，特别是对于复杂任务或当人类演示有限时。
*   **范围有限：** `IRL` 通常侧重于从动作中推断奖励函数。它可能会忽略观察到的行为未直接捕捉到的更广泛的人类偏好或价值观。

### 直接反馈整合示例

要将直接人类反馈整合到 RL 系统中，至少有三种最常用的实践方法：

*   **奖励标注**：人类直接为不同的状态或动作打上正向、负向或中性的奖励标签。
*   **偏好学习**：人类表达对智能体生成的不同输出的偏好，引导智能体朝向更理想的行为。
*   **评价性反馈**：人类对智能体的输出提供书面或口头评价，随后可用于优化奖励信号。

总之，RLHF 通常倾向于直接反馈整合，因为它允许以清晰、高效且灵活的方式将人类价值观融入训练过程。虽然 `IRL` 提供了有价值的见解，但直接利用人类偏好的能力通常能带来更稳健、更符合人类意图的 RL 智能体。但请注意，在某些应用中，即时收集人类偏好可能不可行（例如实时动态场景），必须依赖人类演示，例如自动驾驶中，可以从驾驶演示中获取人类的动作和行为。

### 收集人类反馈

人类反馈的数据收集可能成本高昂，且需要特定于应用的系统。为了在本章中进行演示，我们创建了模拟的状态-动作场景，并提供代表人类反馈的模拟奖励。我们复用了*第 1 章*中模拟器环境的 `GridWorld` 模拟类，以及上一节中的 `RewardModel` 类，它们展示在仓库的 `reward_modeling.py` 文件中。我们还将展示如何为了演示目的模拟人类反馈。导入奖励建模所需的必要库和其他依赖项，如下所示。

```python
import numpy as np
import torch
import torch.nn as nn
import torch.optim as optim
from gridworld import GridWorld
from reward_models import RewardModel
```




## 生成人类反馈

让我们开发一些函数，以帮助生成具有代表性的人类反馈数据。首先，我们编写一个函数，它接收智能体的当前状态、采取行动后的下一状态以及环境信息（目标位置和障碍物）作为输入。然后，它计算三个奖励组件，模拟人类可能如何评估智能体的进展：

### 进度奖励

该奖励激励智能体向目标靠近。它计算下一状态与目标位置之间的距离。智能体越接近目标，奖励越高。这是通过使用 `np.linalg.norm` 函数计算下一状态与目标位置之间的欧几里得距离，然后乘以 -1 来实现的（因为距离变小意味着靠近目标）。

### 安全奖励

该奖励阻止智能体进入障碍物位置。如果下一状态与环境中的任意障碍物位置（在 `obstacle_positions` 中提供）重合，则给予 -5 的惩罚。如果下一状态是安全的（非障碍物），则奖励为 0。

### 目标效率奖励

该奖励鼓励智能体直接到达目标。如果智能体到达目标状态（根据 `np.array_equal` 判断），则给予高额奖励（+10），否则给予 -1 的惩罚。

通过组合这些奖励，该函数提供了一个综合信号，反映了智能体的期望行为：靠近目标（正向进度奖励）、避开障碍物（正向安全奖励）以及高效到达目标（正向目标效率奖励）。然而，该奖励模型允许将这三个奖励分别处理，并在更高层级上提供更多控制，以决定如何组合这些奖励。这类似于多目标奖励模型，允许对轨迹的每一步进行奖励评分。

```python
def simulate_human_reward(state, next_state, goal_position, obstacle_positions):
    next_state_array = np.array(next_state)
    goal_position_array = np.array(goal_position)
    progress_reward = -np.linalg.norm(next_state_array - goal_position_array)
    safety_reward = -5 if next_state in obstacle_positions else 0
    goal_efficiency_reward = 10 if np.array_equal(next_state_array, goal_position_array) else -1
    return np.array([progress_reward, safety_reward, goal_efficiency_reward])
```

`generate_human_feedback` 函数为网格世界环境中的各种状态转换生成模拟人类反馈数据集。它接收网格世界的大小、目标位置、障碍物位置和期望的样本数量（`num_samples`）作为输入。它还接收智能体在网格世界环境中可采取的可能动作：向上、向下、向左或向右（由元组表示，如向上为 (0, 1)，`actions = [(0, 1), (0, -1), (1, 0), (-1, 0)]`），并初始化一个空列表 `data` 以存储生成的反馈样本。它循环 `num_samples` 次，在网格世界边界内随机采样一个起始状态（智能体位置），从可用动作中选择一个动作，并计算智能体采取该动作后将到达的下一状态。一旦拥有当前状态、下一状态和环境信息（目标和障碍物），它就会调用 `simulate_human_reward` 函数，获取该特定状态转换对应的类人奖励。它将一个包含当前状态、下一状态和计算出的奖励（进度、安全和目标效率）的元组追加到 `data` 列表中。该函数返回生成的数据集 `data`，作为 `RewardModel` 的训练样本。该模拟反馈数据集为 `RewardModel` 提供了多种学习场景。通过在训练期间引入这些奖励，模型学会将状态转换与人类偏好的结果相关联，从而引导 RL 智能体表现出在给定环境中被认为是理想的行为。

## 训练奖励模型

`train_reward_model` 函数接收模拟的人类反馈数据（前面生成的 `data`），并训练 `RewardModel` 类以准确预测这些奖励，如仓库中的 `reward_modeling.py` 文件所示，也在 `train_reward_model` 函数片段中给出，如下所示。

```python
def train_reward_model(data, state_size=2, hidden_size=64, output_size=2, num_epochs=100, batch_size=64, learning_rate=0.001):
    reward_model = RewardModel(state_size, hidden_size, output_size)
    optimizer = optim.Adam(reward_model.parameters(), lr=learning_rate)
    criterion = nn.MSELoss()
    for epoch in range(num_epochs):
        np.random.shuffle(data)
        batch_losses = []
        for i in range(0, len(data), batch_size):
            batch = data[i:i + batch_size]
            states = torch.tensor([item[0] for item in batch], dtype=torch.float32)
            next_states = torch.tensor([item[1] for item in batch], dtype=torch.float32)
            rewards = torch.tensor([item[2] for item in batch], dtype=torch.float32)
            optimizer.zero_grad()
            predictions = reward_model(states, next_states)
            loss = criterion(predictions, rewards)
            loss.backward()
            optimizer.step()
            batch_losses.append(loss.item())
        avg_loss = np.mean(batch_losses)
        print(f"Epoch {epoch+1}/{num_epochs}, Loss: {avg_loss:.4f}")
    return reward_model
```

训练过程可分为以下三个步骤：

### 设置模型和训练环境

它创建 `RewardModel` 类的一个实例，指定状态大小（基于环境表示）、隐藏层大小（控制模型复杂度）和输出大小（预测奖励的数量——本例中为 3 个，结合了进度、安全和目标效率）。它定义一个优化器（`optim.Adam`），用于在训练期间更新模型的权重。该优化器使用学习率（`learning_rate`）来控制这些更新的步长。它定义一个损失函数（`nn.MSELoss`）。该函数测量模型预测的奖励（`predictions`）与人类反馈数据中提供的实际奖励（`rewards`）之间的均方误差（`MSE`）。最小化此 `MSE` 损失鼓励模型做出与人类提供的奖励紧密匹配的预测。





## 训练奖励模型

### 训练循环

它会迭代指定数量的轮次（`num_epochs`）。每个轮次代表对整个训练数据集的一次完整遍历。在每个轮次内，它使用 `np.random.shuffle` 打乱数据（`data`），以避免训练过程中的偏差。这确保模型在整个训练过程中以不同的顺序看到来自整个数据集的样本。它以小批量（`batch_size`）处理数据。批量处理通过更好地利用硬件资源来提高训练效率。在这里，它按 `batch_size` 的大小分块遍历数据。对于每个批量，它从当前批量中提取状态、下一状态和对应的奖励。它将这些数据点转换为 PyTorch 张量——状态和下一状态被转换为浮点张量（`dtype=torch.float32`），因为模型期望数值输入。奖励也被转换为张量以进行高效计算。它在每次更新前将优化器（`optimizer.zero_grad()`）中累积的梯度清零。这是为每一步的反向传播做准备所必需的。然后它将状态和下一状态张量输入 `RewardModel`，以获取模型对当前状态转移的预测奖励。使用预测奖励，它利用损失函数计算预测奖励（`predictions`）与实际奖励（`rewards`）之间的均方误差（MSE）损失，并执行反向传播。计算出的损失（`loss`）通过网络反向传播，以计算损失相对于每个模型参数的梯度。然后，它根据计算出的梯度使用优化器（`optimizer.step()`）更新模型的权重。这一步调整模型的内部参数以减少损失并提高其预测准确性。它还会跟踪每个批量的损失并将其追加到列表 `batch_losses` 中。在遍历完一个轮次中的所有批量后，它计算该轮次的平均损失（`avg_loss`）。然后，它打印当前轮次编号、总轮次数和该轮次的平均损失。这提供了关于训练进度的信息。

### 返回训练好的模型

完成所有轮次后，函数返回训练好的 `reward_model`。该模型已学会根据模拟的人类反馈数据预测奖励。随后，它可被 RL 智能体用于指导其在实际环境中的决策过程。

通过使用模拟人类反馈进行迭代训练，`RewardModel` 学习了状态转移与期望奖励之间的关系。这种知识使模型能够预测未见状态的奖励，从而让 RL 智能体能够在环境中导航并实现人类认为有利的目标。

### 主执行逻辑

脚本的主执行逻辑如下所示。

```python
if __name__ == "__main__":
    grid_size = (6, 6)
    goal_position = (5, 5)
    obstacle_positions = [(2, 3), (3, 1), (4, 3)]
    gridworld = GridWorld(grid_size, obstacle_positions)
    actions = gridworld.actions
    # Generate human feedback and train the reward model
    human_feedback_data = generate_human_feedback(grid_size, actions, goal_position, obstacle_positions, num_samples=500)
    reward_model = train_reward_model(human_feedback_data, state_size=2, hidden_size=64, output_size=3, num_epochs=5000)
    # Save the trained model
    torch.save(reward_model.state_dict(), "reward_model.pth")
    print("Reward model saved to reward_model.pth")
```

它本质上演示了如何使用定义的函数（`generate_human_feedback` 和 `train_reward_model`）为给定的网格世界环境创建模拟人类反馈数据并训练奖励模型。`generate_human_feedback` 函数可以通过一个随机函数来表示，该函数通过调用 `simulate_human_reward` 函数生成状态、下一状态和对应的奖励，如下片段所示：

```python
def generate_human_feedback(grid_size, actions, goal_position, obstacle_positions, num_samples):
    # actions = [(0, 1), (0, -1), (1, 0), (-1, 0)]
    data = []
    for _ in range(num_samples):
        state = (np.random.randint(0, grid_size[0]), np.random.randint(0, grid_size[1]))
        action = actions[np.random.choice(len(actions))]
        next_state = (state[0] + action[0], state[1] + action[1])
        rewards = simulate_human_reward(state, next_state, goal_position, obstacle_positions)
        data.append((state, next_state, rewards))
    return data
```

生成人类奖励并训练奖励模型后，主执行代码使用 `torch.save` 函数将训练好的 `reward_model` 的内部状态字典（`state_dict()`）保存到名为 `reward_model.pth` 的文件中。该文件捕获了模型学习到的权重和偏置，允许稍后加载并使用。训练好的模型随即被保存，以供 RL 智能体将来潜在使用。

## 与 RL 策略训练集成

为了让智能体学习到期望的策略，在上一节讨论的 **Q-learning** 算法中，我们加载并将训练好的奖励模型与网格世界示例的 Q-learning 实现相集成。请注意，此处使用 Q-learning 仅作为示例用例；我们将在后续章节讨论更多其他合适的算法。在下面的步骤中，我们将展示如何将上一节训练的奖励模型与 Q-learning RL 算法集成以进行策略训练。

### 具有基于神经网络奖励模型的子类

`GridWorldWithNNReward` 类继承自 `GridWorld` 类，并增加了使用训练好的奖励模型的功能。首先，它调用父类构造函数以设置网格世界环境。它使用 `load_reward_model` 函数从指定路径（`reward_model_path`）加载预训练的奖励模型。它将加载的模型设置为评估模式（`model.eval()`），以在预测期间禁用 dropout 层（如果使用）。

```python
# Subclass with Neural Network-based Reward Model
class GridWorldWithNNReward(GridWorld):
    def __init__(self, grid_size, obstacle_positions, reward_model_path):
        super().__init__(grid_size, obstacle_positions)
        self.reward_model = self.load_reward_model(reward_model_path)
        self.reward_model.eval()
```

### 加载奖励模型

`load_reward_model` 函数创建一个新的 `RewardModel` 实例，其架构与训练好的模型相同（`state_size`、`hidden_size`、`output_size`）。它使用 `torch.load` 从保存的模型文件（`reward_model.pth`）加载权重和偏置。然后，它返回加载的模型。

```python
    def load_reward_model(self, path):
        model = RewardModel(state_size=2, hidden_size=64, output_size=3)
        model.load_state_dict(torch.load(path))
        return model
```

### 结合神经网络奖励的更新版 Q-Learning

之前的 `q_learning` 函数已更新，集成了基于神经网络的奖励模型，即 `q_learning_with_nn_reward`。详细函数可在 Chapter 3 的代码仓库中找到，文件名为 `q_learning_rm.py`。更改涉及将状态和下一状态转换为张量。然后使用加载的训练好的奖励模型，输入张量状态和下一状态，以获取进度、安全性和目标效率的预测奖励。它将这些组合成单个奖励值，并将奖励值裁剪到定义的范围内（例如 -5 到 10），以避免极端异常值。

```python
tensor_state = torch.tensor(state, dtype=torch.float32).unsqueeze(0)
tensor_next_state = torch.tensor(next_state, dtype=torch.float32).unsqueeze(0)
rewards_nn = self.reward_model(tensor_state, tensor_next_state)
progress_reward, safety_reward, goal_efficiency_reward = rewards_nn.squeeze(0).detach().numpy()
reward = progress_reward + safety_reward + goal_efficiency_reward
reward = np.clip(reward, -5, 10)  # Clip reward
```




主要执行块如下。它定义了网格世界环境参数（大小、目标、障碍物）和已保存奖励模型的路径。它创建了一个 `GridWorldWithNNReward` 实例，该实例集成了预训练的奖励模型。然后它调用 `q_learning_with_nn_reward` 函数，使用基于神经网络的奖励模型运行 Q-Learning 算法。它从函数的返回值中检索收集的数据（奖励、步数和策略进度）。然后它调用绘图函数（类似于之前讨论的）来可视化学习曲线（奖励）、每回合步数以及随时间变化的策略进度。

```python
if __name__ == "__main__":
    grid_size = (6, 6)
    goal_position = (5, 5)
    obstacle_positions = [(2, 3), (3, 1), (4, 3)]
    reward_model_path = "reward_model.pth"
    # Initialize GridWorld with NN-based reward model
    grid_world_nn = GridWorldWithNNReward(grid_size, obstacle_positions, reward_model_path)
    # Run Q-learning with neural network-based reward model
    rewards, steps_per_episode, policy_progress = grid_world_nn.q_learning_with_nn_reward(num_episodes=500, learning_rate=0.1, discount_factor=0.95, epsilon=0.1)
    # Plot the learning curve, steps per episode, and policy progress
    grid_world_nn.plot_rewards(rewards)
    grid_world_nn.plot_steps_per_episode(steps_per_episode)
    grid_world_nn.plot_policy_progress(policy_progress)
```

该实现展示了如何将捕捉人类偏好的训练奖励模型（通过模拟奖励）无缝集成到 Q-Learning 算法中。奖励模型通过提供超越传统稀疏奖励的信息性奖励信号来引导智能体的学习过程。读者可以尝试不同的网格大小、障碍物数量、初始位置和目标位置的变化，以及奖励模型训练中所需的相应变化，如训练轮数和轨迹收集。在上述实现中，奖励模型被构建为预测每次迭代的奖励，而不是整个轨迹的奖励。对于网格世界导航等系统，轨迹的每一步可能有多种可能的结果，除非可能的选择也受到限制，否则仅提供两个偏好选择可能是有限的。绝对评分奖励建模更适合此类情况。我们将在后续章节中重新讨论基于偏好的建模，那时偏好选择由预训练语言模型生成，提供偏好选择来学习此类模型更为合适。

### 总结

在本章中，我们讨论了奖励建模的概念、奖励在强化学习中的重要性、传统强化学习中设计奖励函数的挑战、奖励建模的各种技术，以及如何将人类反馈纳入奖励模型并将奖励模型融入强化学习。奖励建模通过预测状态转移的奖励值来帮助训练强化学习智能体。该值超越了到达目标等简单奖励，结合了人类对进度、安全性和效率等因素的偏好。训练涉及向模型输入模拟或真实的人类反馈数据，使其学习状态转移与期望奖励之间的联系。来自奖励模型的这种更丰富的奖励信号引导强化学习智能体采取最大化长期收益的行动，使其行为与人类期望保持一致。通过在网格世界导航案例上的实现演示，我们讨论了奖励建模的选择取决于所关注的问题类型。在下一章中，我们将讨论一个不同于网格世界导航的更复杂用例，以及各种能够扩展到连续状态和动作的最先进算法。我们还将展示如何通过行为克隆利用人类指导加速训练，这是另一种融入人类反馈的方式。

### 参考文献

列出了可帮助读者深入了解该主题的强化学习书籍、课程、论文和链接：

*   面向强化学习智能体的可解释奖励设计：[`proceedings.neurips.cc/paper/2021/file/a7f0d2b95c60161b3f3c82f764b1d1c9-Paper.pdf`](https://proceedings.neurips.cc/paper/2021/file/a7f0d2b95c60161b3f3c82f764b1d1c9-Paper.pdf)
*   定义和表征奖励黑客：[`arxiv.org/pdf/2209.13085`](https://arxiv.org/pdf/2209.13085)
*   最大熵逆强化学习 ([aaai.org](https://aaai.org))：[`cdn.aaai.org/AAAI/2008/AAAI08-227.pdf`](https://cdn.aaai.org/AAAI/2008/AAAI08-227.pdf)
*   通过多目标奖励建模和专家混合实现可解释偏好：[`arxiv.org/abs/2406.12845`](https://arxiv.org/abs/2406.12845)
*   基于偏好的强化学习方法综述：[`www.jmlr.org/papers/volume18/16-634/16-634.pdf`](https://www.jmlr.org/papers/volume18/16-634/16-634.pdf)
*   来自人类偏好的深度强化学习：([`arxiv.org/abs/1706.03741`](https://arxiv.org/abs/1706.03741))
*   不完全区组设计的秩分析：I. JSTOR 上的成对比较法：[`www.jstor.org/stable/2334029?origin=crossref`](https://www.jstor.org/stable/2334029?origin=crossref)

本章代码示例中使用的库的文档和代码文档如下。读者可用于了解代码实现和可视化方法的更多信息：

*   [Matplotlib 文档 — Matplotlib 3.8.4 文档](https://Matplotlib)
*   [`numpy.org/devdocs/user/index.xhtml`](https://numpy.org/devdocs/user/index.xhtml)
*   [`pytorch.org/docs/stable/index.xhtml`](https://pytorch.org/docs/stable/index.xhtml)

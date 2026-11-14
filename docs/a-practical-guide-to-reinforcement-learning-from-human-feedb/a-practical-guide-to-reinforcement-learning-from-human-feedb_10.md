## 第 10 章：直接偏好优化

### 加入我们的图书社区 Discord

![中间带有一个正方形的二维码，描述自动生成](img/file0.png)

[`https://packt.link/EarlyAccessCommunity`](https://packt.link/EarlyAccessCommunity)

到目前为止，您已经熟悉了将 AI 模型与人类偏好对齐的主要标准方法，主要是 `RLHF`、`RLAIF` 和 `Constitutional AI`。我们展示了构建奖励模型是 `RLHF` 中的关键步骤，如果奖励模型训练不佳，可能会导致一些局限性。在本章中，我们将探索一类近期引入的不同方法，统称为 `Direct Alignment from Preferences` (`DAP`，直接偏好对齐)，其中 `Direct Preference Optimization` (`DPO`，直接偏好优化) 是主要方法。这些方法不需要奖励模型，因此不受奖励模型错误或奖励黑客攻击等问题的影响。主要主题包括：

*   模型可控性与传统 `RLHF` 的局限
*   `DPO` 的出现
*   `DPO` 流程
*   `DPO` 的实现
*   超越 `DPO`：其他直接对齐方法
*   局限性与实际考量

### 技术要求

对于本章，我们将所有与实现相关的依赖项保持最小化。所有实现均使用 `python`。具体的版本建议和需要安装的最低要求库如下：

*   `Python version >= 3.9`
*   `peft == 0.15.2`
*   `transformers == 4.51.3`
*   `accelerate == 1.7.0`
*   `ipython == 8.27.0`
*   `ipywidgets == 8.1.5`
*   `trl == 0.17.0`
*   `bitsandbytes == 0.45.5`

除了上述库和安装之外，我们还将使用 Google Colab 进行基于 GPU 的微调（稍后详述）。要开始，请克隆此 git 仓库：

```
git clone https://github.com/PacktPublishing/A-Practical-Guide-to-Reinforcement-Learning-from-Human-Feedback
```

并将目录切换到 `` `chapter 10` ``。

## 模型可控性与传统 RLHF 的局限性




# 基于人类反馈的强化学习 (RLHF) 与直接偏好优化 (DPO)

`RLHF` 流水线通常结合 `SFT`、奖励建模和策略优化——常使用 `PPO`。虽然这种模块化设计允许分阶段评估和调试，但也引入了显著的复杂性和脆弱性。在人工标注数据上进行 `SFT` 可提升模型对特定指令的响应能力，但也可能导致过拟合、降低对新提示词的泛化能力，甚至退化预训练能力——表现为事实性错误、幻觉或安全指标下降。为解决这些缺陷，`RLHF` 引入了基于人类偏好比较训练的奖励模型，使系统能区分优劣模型响应。随后对策略进行优化，以生成符合高评分偏好的输出。然而，奖励模型自身易过拟合于狭窄的训练分布、误判语义等价的改写，并激励非预期行为——通常称为*奖励黑客攻击*。`PPO` 因对奖励缩放、`KL` 惩罚、超参数调整和分布偏移敏感，进一步增加了复杂性。随着训练数据演进，重新训练成本增长，尤其在需要频繁更新以防止错位和奖励利用时。

尽管存在局限，`RLHF` 仍是引导大语言模型朝期望行为发展的基础技术。通过将模型输出与人类偏好对齐，`RLHF` 增强了模型的可控性——即模型根据特定用户目标或指令调整其响应的能力，可通过其满足特定任务或用户定义标准的程度来衡量。论文《可生成的未必可达：测量生成模型的可控性》（https://arxiv.org/abs/2503.17482v1）研究了各种模型以测量可控性是否可实现。论文发现，在没有刻意优化的情况下，引导模型生成期望响应要困难得多。相比之下，让模型仅生成响应（可生成性）则更容易。虽然有效，但基于 `RLHF` 的方法引入了奖励错位、训练不稳定和资源密集等挑战。这些缺点激发了对替代对齐策略的兴趣，这些策略在适应不断变化的用户需求时提供更高的样本效率、鲁棒性和灵活性。

一种极具前景的替代方案是 `DPO`，它属于称为 `DAP` 的更广泛方法类别。这些方法绕过了对显式奖励模型和强化学习的需求，直接从成对人类偏好数据优化策略。通过简化对齐流水线，`DPO` 解决了 `RLHF` 固有的许多脆弱性——最显著的是奖励模型不准确性和 `PPO` 等策略优化算法的不稳定性。在下一节中，我们将探讨 `DPO` 的概念基础，以及它如何重新思考从偏好数据到可控模型行为的路径。

## 直接偏好优化的涌现

传统 `RLHF` 的局限性——特别是其对脆弱奖励模型和不稳定强化学习流水线的依赖——为范式转变创造了沃土。`DPO` 的出现不仅是技术调整，更是概念突破，将对齐重构为直接策略优化问题，而非间接的奖励驱动任务。

到 2023 年，不断积累的证据揭示了 `RLHF` 的痛点。正如论文《大语言模型 RLHF 的奥秘 第二部分：奖励建模》（https://arxiv.org/html/2401.06080v2）所讨论，奖励模型往往无法泛化到其训练分布之外。它们引入了奖励黑客攻击和计算瓶颈等挑战，详见《直接偏好优化：你的语言模型秘密地是一个奖励模型》（https://arxiv.org/abs/2305.18290）。研究者开始探索这样一个假设：通过显式奖励函数建模人类偏好可能是不必要的。策略本身可以通过奖励-策略关系的巧妙重参数化直接内化偏好。这一洞察绕过了单独奖励建模和 `RL` 微调的需求，将对齐压缩为单一分类目标。

这种实践痛点启发了一个基础问题：是否真的有必要通过显式奖励函数来建模偏好？研究者假设策略本身可以通过奖励-策略关系的巧妙重参数化直接内化偏好。这一洞察绕过了奖励模型和单独 `RL` 微调阶段的需求——将两者合并为基于分类的单一目标。

`DPO` 提供了一个简单而强大的想法：给定一个提示词和两个响应（一个偏好、一个拒绝），微调模型以增加偏好响应的似然并降低拒绝响应的似然。关键在于，优化针对冻结参考模型进行正则化，锚定新策略，并消除了 `RLHF` 流水线为防止奖励过度优化和模型漂移所需的精心调整 `KL` 散度惩罚。

`DPO` 的初步实验展示了惊人结果：在情感控制和摘要任务中，其性能匹配或超越 `RLHF`，所需算力却减少 3 倍。关键在于，`DPO` 对分布偏移更具鲁棒性，因为它避免了不完美奖励模型的误差累积。这些发现催化了快速采用，Hugging Face 的 `TRL` 等框架在其提出几个月内就集成了 `DPO`。

`DPO` 属于不断增长的 `DAP` 方法类别，例如《来自在线 AI 反馈的直接语言模型对齐》（https://arxiv.org/abs/2402.04792）和《面向异构偏好的直接对齐》（https://arxiv.org/html/2502.16320v1）中讨论的方法，它们优先考虑端到端优化。`RLHF` 将对齐分解为奖励学习和策略优化，而 `DAP` 方法将偏好数据视为直接监督信号——这一转变映射了自监督学习的趋势。这种方法更好地适应了人类价值观的多面性，避免了将偏好压缩为标量奖励的有损压缩。

正如自监督学习消除了表征学习中对标注数据的需求，`DAP` 方法挑战了对齐中奖励模型的必要性。`DPO` 不将丰富、主观的偏好压缩为标量奖励值，而是允许模型在对比框架中直接建模偏好关系——保留了人类反馈更多的结构和歧义。

`DPO` 的涌现标志着一个转折点，证明了 `RLHF` 的复杂性并非不可避免。其成功验证了一种新设计哲学：当建立在策略建模偏好的固有能力之上时，对齐可以既更简单又更有效。在下一节中，我们将详述 `DPO` 流水线并演示其过程。

## DPO 流水线

`DPO` 通过将传统 `RLHF` 的奖励建模和策略优化阶段精简为植根于偏好比较的单一优雅目标，转变了基于偏好的对齐范式。本节详述 `DPO` 作为流水线如何工作，如何直接利用人类偏好塑造策略，以及其损失函数如何为偏好学习提供 principled（原则性的/有原则的）概率框架。

### DPO 概览




# 直接偏好优化 (DPO)

**直接偏好优化** (**DPO**) 代表了语言模型对齐的重大进展，为传统的**基于人类反馈的强化学习** (**RLHF**) 提供了更优雅高效的替代方案。其核心在于，DPO 直接从成对的人类偏好数据中学习——绕过了对显式奖励模型或复杂强化学习机制的需求。DPO 背后的基本洞察直观明了：当面对一个提示词的两个回复——一个被人类偏好，一个被拒绝——模型应更新以增加生成偏好回复的概率，同时降低生成被拒回复的可能性。这是通过对比学习框架实现的，该框架将偏好比较视为二分类问题。

DPO 训练流程简洁高效：

- **数据收集**：人类标注员以三元组形式提供反馈：(提示词, 偏好回复, 被拒回复)。
- **模型初始化**：策略模型从预训练检查点初始化，通常是经过**监督微调** (**SFT**) 的检查点。
- **参考模型锚定**：使用初始模型的冻结副本作为参考以正则化更新，防止策略偏离其基础行为过远。
- **对比偏好优化**：模型使用专门的损失函数进行微调，鼓励其给偏好回复分配比被拒回复更高的对数似然。

DPO 的关键创新之一是将 RLHF 中的奖励最大化目标重构为针对人类偏好的更易处理的二分类任务。这种重构使得训练目标更易实现、优化更稳定，且在实践中通常更高效。

## 人类偏好如何直接塑造策略

在传统 RLHF 中，人类偏好首先通过学习到的奖励模型转换为标量奖励值。然后策略通过强化学习训练以最大化期望回报。**直接偏好优化 (DPO)** 完全移除了这一中间步骤。相反，它直接利用偏好数据，将成对比较视为**对比监督信号**。

对于每个提示词，人类标注员在两个候选回复之间提供偏好：**偏好回复** 和 **被拒回复**。目标是更新策略 `π`，使其给偏好回复分配更高的似然。DPO 不显式强制这一不等式，而是通过源自两个回复对数似然的损失函数来实现。典型形式使用逻辑 (sigmoid) 变换来表达偏好概率：

此处，`π_ref` 是**冻结的参考模型**（通常是监督微调检查点），`β` 是控制偏好信号锐度的温度参数。参考对数概率的相减引入了**KL 散度正则化**效应：它隐式惩罚新策略偏离参考模型过远。这种正则化将更新锚定在已知的良好行为上，稳定训练。因此，策略被鼓励仅在收益*仅当收益足够显著时*才改进人类偏好——在偏好对齐与输出稳定性及多样性之间取得平衡。

## 损失函数直觉

DPO 损失函数的灵感源于偏好的玻尔兹曼理性模型下的最大似然原则。其基本假设是人类偏好可概率建模：偏好某回复胜过另一回复的可能性与二者在策略下的对数似然得分之差的指数成正比。

给定一个提示词和两个回复——**偏好回复** `y_w` 和 **被拒回复** `y_l`——人类偏好 `y_w` 而非 `y_l` 的概率建模为：

此处，`β` 是控制偏好执行锐度的温度参数。损失定义为观测到的人类偏好的负对数似然：

其中：
- 这种参考模型对数概率的相减引入了**隐式 KL 正则化**。

由此产生的目标确保策略仅在偏好要求的地方改进，保持相对于参考模型的保守性。这种方法在概念上与**保守策略优化**一致——一种旨在将策略更新约束在基线周围信任区域内的技术。从变分角度看，DPO 可解释为最小化策略与由偏好塑造的目标分布之间的**对比 KL 散度**，同时利用参考模型锚定更新。

> **注意：**
>
> 该损失通常使用带有 sigmoid 变换的二元交叉熵公式实现，符合标准深度学习实践。Sigmoid 函数的使用提供了数值稳定性，并自然契合偏好的概率解释。

DPO 流程通过直接在人类偏好上优化来简化对齐，绕过了对奖励模型的需求。核心概念就位后，我们现在转向实现——展示如何预处理偏好数据并使用 DPO 损失训练模型。

## DPO 实现

本节我们将演示实现 DPO 的实际步骤。延续前几章的方法，我们使用 Hugging Face `trl` 库并配合 Colab 笔记本设置，以保证可复现性和实验便利性。GPU 设置请参考*第 8 章*，或沿用前几章的设置。你可以在本地环境安装所有必需包（见本章开头列出的要求），或直接在笔记本中运行：

```
!pip install -U -q peft==0.15.2 transformers==4.51.3 accelerate==1.7.0 trl==0.17.0 bitsandbytes==0.45.5
```

在示例实现中，我们使用 `Qwen2-0.5B-Instruct` 模型作为基础策略。这个紧凑的指令微调模型在计算效率和表达能力之间提供了实用的平衡。其适中的规模（5 亿参数）使读者能在有限硬件（如单 GPU 或 Google Colab 实例）上进行实验，同时仍能实现有意义的偏好驱动更新。与 Mistral 或 LLaMA-2 等替代方案相比，Qwen2 以开放权重形式提供强大的多语言能力和稳健的预训练目标，成为可复现对齐实验的吸引人选择。

### 模型与量化设置

在示例中，我们使用 `Qwen2-0.5B-Instruct` 模型，一个紧凑的指令微调因果语言模型。为在单 GPU 或 Google Colab 等有限硬件上进行训练和推理，我们使用 `bitsandbytes` 库提供的 4 位量化加载模型。此方法大幅减少内存占用并加快模型加载。虽然量化不改变 DPO 算法本身，但它反映了人类对齐训练工作流中遇到的真实硬件约束。完整代码详情请参阅 GitHub 仓库中的配套笔记本：[`github.com/PacktPublishing/A-Practical-Guide-to-Reinforcement-Learning-from-Human-Feedback`](https://github.com/PacktPublishing/A-Practical-Guide-to-Reinforcement-Learning-from-Human-Feedback) （导航至*第 10 章*）。

### 应用 LoRA 进行高效微调

正如*第 6 章*所述，使用 LoRA 适配器的**参数高效微调** (**PEFT**) 对于在 DPO 期间高效适配大模型至关重要。关键步骤是指定应用 LoRA 的模型正确内部层。对于 `Qwen2` 模型，目标模块包括：



## 为量化 Qwen2 模型配置 LoRA

以下代码片段展示了如何为量化 Qwen2 模型配置并应用 LoRA：

```python
target_modules = [
    "q_proj", "k_proj", "v_proj", "o_proj",
    "gate_proj", "up_proj", "down_proj"
]
```

```python
from peft import LoraConfig, get_peft_model, TaskType
peft_config = LoraConfig(
    r=8,
    lora_alpha=16,
    target_modules=target_modules,
    lora_dropout=0.05,
    bias="none",
    task_type=TaskType.CAUSAL_LM
)
model = get_peft_model(model, peft_config)
```

> **注意：** 请确保 `target_modules` 与基础模型的内部层名称完全一致，以避免错误。有关检查模型层的指导，请参阅 GitHub 仓库中的笔记本。

## 加载和预处理偏好数据

DPO 算法需要成对的人类偏好示例——具体来说，是提示词-补全对，其中一个回复比另一个更受青睐。我们使用 UltraFeedback Binarized 数据集的一个小子集进行演示。该数据集包含针对同一提示词的首选回复和非首选回复。我们首先加载数据集并对其进行构建以供后续使用：

```python
from datasets import load_dataset
# Load a small sample for testing
dataset = load_dataset("trl-lib/ultrafeedback_binarized", split="train[:100]")
```

接下来，我们将数据集拆分为训练集和评估集。这使我们能够在微调过程中衡量模型的泛化性能。

```python
split_dataset = dataset.train_test_split(test_size=0.2)
train_dataset = split_dataset["train"]
eval_dataset = split_dataset["test"]
```

## 对偏好数据进行分词

要训练基于 Transformer 的语言模型，我们需要对输入文本进行分词。我们使用与 Qwen2-0.5B-Instruct 模型关联的分词器。为简单起见并便于诊断，我们在此步骤中将 chosen 和 rejected 回复拼接在一起。这有助于确认分词器的行为符合预期。稍后，DPO 训练器将分别处理提示词和回复。

```python
from transformers import AutoTokenizer
# Load the tokenizer
tokenizer = AutoTokenizer.from_pretrained("Qwen/Qwen2-0.5B-Instruct")
# Tokenization: concatenate chosen and rejected for a sanity check
def preprocess_function(examples):
    input_text = [f"Chosen: {ch} Rejected: {rej}" for ch, rej in zip(examples['chosen'], examples['rejected'])]
    return tokenizer(input_text, padding=True, truncation=True, max_length=512)
# Apply preprocessing
tokenized_train = train_dataset.map(preprocess_function, batched=True)
tokenized_eval = eval_dataset.map(preprocess_function, batched=True)
```

> **注意：** 拼接格式（"Chosen: ... Rejected: ..."）是用于验证的占位符。在 DPO 训练期间，模型会分别接收提示词、chosen 回复和 rejected 回复作为输入。这由 `DPOTrainer` 自动处理。

## 配置 DPO 训练

`TRL` 库包含一个高层 `DPOTrainer` 类，它需要一个 `DPOConfig` 对象来指定训练超参数。这里，我们定义了一个最小化的训练配置，适用于在小型 GPU 或 Google Colab 等云环境中进行原型设计。

```python
from trl import DPOConfig
dpo_config = DPOConfig(
    output_dir="qwen2-dpo-output",
    per_device_train_batch_size=4,
    gradient_accumulation_steps=4,
    num_train_epochs=3,
    learning_rate=5e-5,
    lr_scheduler_type="linear",
    weight_decay=0.01,
    logging_steps=5,
    report_to="none",  # Disable logging to external tools
    fp16=True           # Use mixed precision if supported
)
```

此配置允许在有限的硬件上进行训练。`gradient_accumulation_steps` 设置允许在不超出内存限制的情况下使用更大的有效批次大小。您可以根据数据规模和实验需要调整 `num_train_epochs` 或 `output_dir` 等参数。在下一节中，我们将以 4-bit 量化形式加载模型，连接 LoRA 适配器（如*第 6 章*所述），并继续使用 `trl` 库中的 `DPOTrainer` 方法初始化 DPO 训练循环。

## 训练偏好优化模型

我们初始化 `DPOTrainer`，并向其提供分词后的训练和评估数据集：

```python
from trl import DPOTrainer
trainer = DPOTrainer(
    model=model,
    ref_model=None,
    args=dpo_config,
    train_dataset=tokenized_train,
    eval_dataset=tokenized_eval,
    processing_class=tokenizer
)
```

`DPOTrainer` 完全配置并初始化后，我们通过调用 `trainer.train()` 开始训练。评估集会在训练期间自动用于记录评估指标。`train()` 方法在我们的分词数据集上启动训练循环。训练完成后，我们保存适配器权重和分词器，以便后续复用或部署。由于我们使用了带有 LoRA 的 `Parameter-Efficient Fine-Tuning` (`PEFT`)（见*第 6 章*），我们只需保存适配器层：

```python
model.save_pretrained("qwen2-lora-adapter")
tokenizer.save_pretrained("qwen2-lora-adapter")
```

`save_pretrained()` 方法将模型配置和适配器权重存储在 `qwen2-lora-adapter` 目录中。对于分词器，保存的文件包括：

```
tokenizer_config.json
special_tokens_map.json
vocab.json
merges.txt
added_tokens.json
tokenizer.json
```

这些文件保留了词汇表、特殊标记和分词器设置，确保与微调模型的兼容性。如果您稍后需要重新加载微调模型，请首先加载基础模型，特别是在新会话或新笔记本中运行时。

```python
from peft import PeftModel
from transformers import AutoTokenizer
import torch
# Reload base model (if not already in memory)
base_model = AutoModelForCausalLM.from_pretrained(
    "Qwen/Qwen2-0.5B-Instruct",
    quantization_config=bnb_config,
    device_map="auto"
)
# Load LoRA adapter from local directory
model = PeftModel.from_pretrained(base_model, "qwen2-lora-adapter")
# Load tokenizer from saved adapter directory
tokenizer = AutoTokenizer.from_pretrained("qwen2-lora-adapter")
```

训练完成或模型重新加载后，我们在保留的评估数据上评估模型性能：

```python
results = trainer.evaluate()
print(results)
```

评估运行的输出是一系列评估指标。这些指标有助于了解训练好的模型与人类偏好的对齐程度：

*   `accuracies`：模型倾向于“chosen”而非“rejected”回复的频率。
*   `margins`：首选回复和非首选回复之间的平均奖励差距。
*   `eval_loss`：模型学习偏好信号效果的代理指标。

既然我们已经演示了经典 DPO 算法的实现——包括偏好数据预处理、分词和训练循环——我们就有能力探索这一直接优化框架是如何被扩展的。几种近期的方法直接建立在 DPO 的基础之上，通过修改其损失函数或正则化策略来提高样本效率、鲁棒性或对齐质量。其他方法则从 DPO 中汲取灵感，但提出了概念上截然不同的替代方案。在下一节中，我们将探讨 DPO 的关键变体以及新兴的直接对齐方法，重点介绍它们的核心创新和实际意义。

## 超越 DPO：其他直接对齐方法




# DPO 的变体

出现了许多变体，它们保留了 DPO 的核心原则——通过对比监督进行直接偏好优化——同时解决了实际局限性。这些方法改进了 DPO 的特定方面，如其损失函数、正则化方案或优化动态。下面，我们将这些变体按主题分类。

## 正则化与约束变体

这些方法修改了 DPO 相对于参考模型约束新策略的方式。

`ORPO` (`Odds-Ratio Preference Optimization`, 几率比偏好优化)：通过将参考模型嵌入几率比公式中，消除了对冻结参考模型的需求，从而降低了计算量和架构复杂性。详见：[`arxiv.org/pdf/2403.07691`](https://arxiv.org/pdf/2403.07691)。

`simPO` (`Similarity-based Preference Optimization`, 基于相似度的偏好优化)：使用模型自身的初始预测作为软基线，允许在保持保守更新的同时进行无参考模型优化。详见：[`arxiv.org/abs/2405.14734`](https://arxiv.org/abs/2405.14734)。

`β-DPO`：在训练期间动态调整 KL 惩罚强度，当偏好信号微弱或嘈杂时增加惩罚，当信号强时减少惩罚。论文 [`arxiv.org/pdf/2407.08639`](https://arxiv.org/pdf/2407.08639) 详细解释了该方法。

## 损失结构增强

这些方法修改 DPO 损失函数以改进优化行为。

`IPO` (`Implicit Preference Optimization`, 隐式偏好优化)：用恒等链接函数替换 Bradley-Terry 变换，并从偏好数据中隐式地引入正则化，减少过拟合并降低对固定参考模型的依赖。详见：[`arxiv.org/pdf/2310.12036`](https://arxiv.org/pdf/2310.12036)。

`CPO` (`Contrastive Preference Optimization`, 对比偏好优化)：向 DPO 损失添加一个监督项，显式增加首选输出的似然，解决了仅靠对比优化可能无法充分强化优胜者的问题。详见：[`arxiv.org/pdf/2401.08417`](https://arxiv.org/pdf/2401.08417)。

`KTO` (`Kahneman–Tversky Optimization`, 卡尼曼-特沃斯基优化)：受 `Prospect Theory`（前景理论）启发，KTO 将对收益和损失的非对称处理引入 DPO 目标，反映了人类在实践中对偏好的不同权重（例如损失厌恶）。详见：[`arxiv.org/abs/2402.01306`](https://arxiv.org/abs/2402.01306)。

## 对齐鲁棒性与多目标优化

这些方法引入正则化或辅助目标以减少对齐伪影。

`R-DPO`：惩罚过度冗长或长度偏好等不良行为，这些行为源于模型学会利用偏好信号。详见：[`aclanthology.org/2024.findings-acl.297.pdf`](https://aclanthology.org/2024.findings-acl.297.pdf)。

`LIFT-DPO`：引入基于指令的正则化，教导模型何时更倾向于较短的回答，提高可控性。详见：遵循指令中的长度约束 [`arxiv.org/html/2406.17744v1`](https://arxiv.org/html/2406.17744v1)。

`MODPO` (`Multi-Objective DPO`, 多目标 DPO)：将 DPO 扩展为联合优化多个偏好维度（例如有用性和无害性），通常通过加权或帕累托优化的方式组合单独的 DPO 目标。详见：超越一偏好通吃对齐：多目标直接偏好优化：[`arxiv.org/abs/2310.03708`](https://arxiv.org/abs/2310.03708)。

`SafeDPO`：在 DPO 过程中将安全视为硬约束，将拒绝行为或内容过滤作为偏好监督的一部分加以强制执行。详见：SafeDPO：一种具有增强安全性的直接偏好优化简单方法 [`openreview.net/forum?id=MoJSnVZ59d`](https://openreview.net/forum?id=MoJSnVZ59d)。

所有这些方法都保留了 DPO 核心的 `contrastive preference modeling`（对比偏好建模）框架，但调整其结构以更好地适应真实世界数据、提高对齐稳定性并扩展其灵活性。它们代表了 DPO 在其概念边界内的演进——而非背离。

# 概念上独特的直接对齐方法

除了 DPO 变体之外，越来越多的直接对齐方法涌现出来，它们共享从人类偏好中学习的目标，但使用不同的建模假设、反馈结构或训练范式来实现。这些方法偏离了原始的 DPO 框架，并定义了基于偏好的微调新方向。

## 列表式偏好优化

这些方法超越了二元成对监督，通过从排序后的响应列表中学习。

`RRHF` (`Reward-Ranked Fine-Tuning`, 奖励排序微调)：训练模型模仿完成列表中排名靠前的输出，使用基于对数似然的模仿而非对比损失。不使用参考模型或 KL 惩罚。详见：[`arxiv.org/abs/2304.05302`](https://arxiv.org/abs/2304.05302)。

`RAFT`：使用列表式目标直接在排序后的输出上进行优化。这种方法鼓励模型学习全局响应质量模式，而不仅仅是局部成对偏好。详见：[`arxiv.org/pdf/2403.10131`](https://arxiv.org/pdf/2403.10131)。

## 无成对偏好学习

一些方法完全消除了对非首选比较的需求。`Unpaired Preference Optimization`（无成对偏好优化）仅使用正面（点赞）反馈，要么将所有其他输出视为隐式负例，要么间接学习奖励函数。这背离了 DPO 的二元结构。详见：[`arxiv.org/pdf/2310.12036`](https://arxiv.org/pdf/2310.12036)。

## 自监督与自我博弈偏好学习

这些方法使用模型自身生成或判断偏好数据。例如，在 `Self-Play Optimization`（自我博弈优化）中，模型生成竞争性输出，一个单独的评判模型（或模型自身）选择更好的一个。训练通过强化获胜策略进行。虽然这可能使用 DPO 风格的优化，但从概念上更接近迭代自我训练或基于比较的模仿。详见：用于语言模型对齐的自我博弈偏好优化：[`arxiv.org/abs/2405.00675`](https://arxiv.org/abs/2405.00675)。

## 细粒度或 Token 级偏好学习




## 直接对齐技术

### 逐步 DPO 式训练

用于思维链提示等场景，其中奖励正确的中间步骤，而不仅仅是最终答案。详见：[sDPO：不要一次性用完你的数据](https://arxiv.org/abs/2403.19270)。

### Token 级偏好监督

该方法仍处于实验阶段，逐词对齐生成，通常需要密集的人工或模型生成的反馈。[Token 级直接偏好优化](https://arxiv.org/pdf/2404.11999)。

这些方法探索新的偏好格式、非对比目标，以及更灵活或高效的训练框架。虽然受 DPO 直接对齐理念启发，但它们代表了概念上的突破，拓展了基于偏好的训练所能实现的范围。DPO 及其变体连同这些更新的、结构上不同的方法，共同构成了一个丰富且不断演进的直接对齐技术空间。理解这些方法的联系与差异，对于根据可用反馈、对齐目标和资源限制选择合适的方法至关重要。

## 局限性与实际考量

尽管具有实际优势，DPO 仍存在若干重要局限。其性能高度依赖偏好数据的质量和一致性，因为它不使用显式奖励塑形来补偿噪声或稀疏信号。在需要结构化信用分配的任务中，如多步推理、长时程决策或细粒度轨迹控制，带有显式奖励模型的 RLHF 可能优于 DPO。这是因为基于奖励的方法可在每个阶段提供连续的标量反馈，使模型能有效学习更细微或分层的行为。此外，DPO 依赖成对比较会降低数据效率，特别是在标注有限或成本高昂时。认识这些约束对于选择合适的对齐策略至关重要。

| 标准 | DPO | 带奖励模型的 RLHF |
| --- | --- | --- |
| 数据类型 | 高质量成对偏好数据 | 偏好数据 + 标量奖励信号 |
| 反馈结构 | 成对比较，标注更简单 | 详细的标量或轨迹级奖励 |
| 任务复杂度 | 单轮、局部、短时程任务 | 多步、长时程、分层任务 |
| 计算实现 | 更简单的流程，无显式奖励模型 | 更复杂，需要训练奖励模型 |
| 噪声敏感度 | 对偏好质量敏感 | 可通过奖励塑形补偿 |
| 样本效率 | 数据稀缺时效率较低 | 密集反馈下更灵活 |
| 对齐控制 | 通过参考模型进行保守更新 | 通过自定义奖励进行细粒度塑形 |

### 总结

本章将 DPO 作为一种可扩展、无需强化学习的方法呈现，用于直接从人类偏好数据对齐语言模型。我们首先通过与传统 RLHF 流程对比来引出 DPO，重点强调其消除了奖励建模阶段，并对偏好和非偏好响应采用对比对数似然目标。我们探讨了 DPO 如何在玻尔兹曼理性假设下建模偏好概率，并引入参考策略通过隐式 KL 散度正则化更新。随后章节详述了 DPO 流水线，涵盖偏好数据收集、基于对数似然的训练目标，以及 DPO 损失背后的直觉。我们完整演示了 DPO 的实现，从三元组数据预处理到配置带参考模型的训练，再到调整温度和批大小等超参数。接着我们综述了一组 DPO 变体，它们保留核心结构，同时修改损失函数、正则化策略或优化动态，以提升泛化、减少过拟合或支持多目标对齐。我们还将这些方法与概念上不同的直接对齐方法区分开来——如列表式模仿、无配对偏好优化和细粒度反馈模型——这些反映了该领域向更灵活、偏好驱动的对齐策略转变的总体趋势。随后简要讨论了 DPO 的局限性以及 RLHF 仍可能是更好替代方案的场景。这些进展共同确立了 DPO 不仅是 RLHF 的实用替代方案，更是基于直接从人类偏好优化的未来对齐方法的基础模板。尽管 DPO 及其扩展为训练对齐感知模型提供了强大工具，但这些模型甚至 RLHF 在多大程度上反映人类偏好，仍有待严格评估才能定论。下一章我们将转向模型评估主题，探讨用于评估对齐质量、泛化能力和行为保真度的自动指标和以人为中心的方法。

### 参考文献

*   [`github.com/huggingface/peft`](https://github.com/huggingface/peft)
*   [`huggingface.co/docs/peft/main/en/index`](https://huggingface.co/docs/peft/main/en/index)
*   [`huggingface.co/docs/trl/sft_trainer`](https://huggingface.co/docs/trl/sft_trainer)
*   ORPO：无参考模型的整体偏好优化 [`arxiv.org/pdf/2403.07691`](https://arxiv.org/pdf/2403.07691)
*   SimPO：基于无参考奖励的简单偏好优化 [`arxiv.org/abs/2405.14734`](https://arxiv.org/abs/2405.14734)
*   对比偏好优化：推动 LLM 在机器翻译中性能的边界 [`arxiv.org/pdf/2401.08417`](https://arxiv.org/pdf/2401.08417)
*   β-DPO：具有动态 β 的直接偏好优化 [`arxiv.org/pdf/2407.08639`](https://arxiv.org/pdf/2407.08639)
*   理解从人类偏好学习的通用理论范式 [`arxiv.org/pdf/2310.12036`](https://arxiv.org/pdf/2310.12036)
*   在直接偏好优化中解耦长度与质量 [`aclanthology.org/2024.findings-acl.297.pdf`](https://aclanthology.org/2024.findings-acl.297.pdf)
*   遵循指令中的长度约束 [`arxiv.org/html/2406.17744v1`](https://arxiv.org/html/2406.17744v1)
*   超越一偏好适配所有对齐：多目标直接偏好优化 [`arxiv.org/abs/2310.03708`](https://arxiv.org/abs/2310.03708)
*   SafeDPO：一种增强安全性的直接偏好优化简单方法 [`openreview.net/forum?id=MoJSnVZ59d`](https://openreview.net/forum?id=MoJSnVZ59d)
*   UPO：面向大语言模型的无配对偏好优化 [`arxiv.org/pdf/2310.12036`](https://arxiv.org/pdf/2310.12036)
*   RRHF：对响应排序以无痛苦地将语言模型与人类反馈对齐 [`arxiv.org/abs/2304.05302`](https://arxiv.org/abs/2304.05302)
*   RAFT：将语言模型适配到特定领域 RAG [`arxiv.org/pdf/2403.10131`](https://arxiv.org/pdf/2403.10131)
*   用于语言模型对齐的自我博弈偏好优化 [`arxiv.org/abs/2405.00675`](https://arxiv.org/abs/2405.00675)

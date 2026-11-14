

```python
from elote import EloCompetitor, Elo
# Initialize ELO system and competitors
elo = Elo(k=32)
model_a = EloCompetitor(rating=1500)
model_b = EloCompetitor(rating=1500)
# Simulate a match where Model A wins
elo.record_match(model_a, model_b, winner=model_a)
print(f"Model A ELO: {model_a.rating}")
print(f"Model B ELO: {model_b.rating}")
```

你可以循环遍历多次成对判断，在每场“比赛”（偏好判断）后更新评分。

# 胜率与 ELO 实验设置

## 胜率
向标注员展示输出对（隐藏模型身份）；记录哪个输出更受青睐。将胜率计算为每个模型被偏好的比例。

## ELO 分数
将每个偏好视为一场“比赛”。使用 ELO 逻辑在每次比较后更新模型评分。像 `elote` 或 `Elo-MMR` 这样的库可以自动完成此过程。

这些指标适用于人工评估和 LLM 作为评判者的实验。

## KL 散度惩罚

`Kullback-Leibler` (`KL`) 散度惩罚衡量的是在 RLHF 训练期间微调模型与其基础参考策略之间的偏差。我们在*第 4 章*使用近端策略优化进行模型训练时也讨论过该指标。通过量化微调策略与参考策略（通常是原始的监督微调模型）之间的分布偏差，该惩罚确保模型在适应人类偏好时不会过度偏离其基础行为。过大的散度可能表明过度优化或 `reward hacking`（奖励黑客），即模型学会利用奖励信号中的不完美之处，生成重复、不连贯或退化的输出。另一方面，过小的散度表明策略过于保守，无法有意义地适应新反馈或提升有用性。KL 惩罚通常直接集成到策略优化期间的奖励函数中，其中 `beta` 控制惩罚的强度。KL 散度计算如下：

```python
import torch.nn.functional as F
def compute_kl_penalty(current_logits, ref_logits, beta=0.1):
    """
    Args:
        current_logits: [batch, seq_len, vocab] from current policy
        ref_logits: [batch, seq_len, vocab] from reference policy
        beta: Penalty scaling coefficient
    Returns:
        Scalar penalty term
    """
    current_probs = F.softmax(current_logits, dim=-1)
    ref_probs = F.softmax(ref_logits, dim=-1)
    kl_div = F.kl_div(
        current_probs.log(),
        ref_probs,
        reduction='batchmean',
        log_target=False
    )
    return beta * kl_div
# compute during training loop
total_reward = preference_reward - compute_kl_penalty(
    current_logits=agent_logits,
    ref_logits=reference_logits,
    beta=0.2
)
```

此示例展示了 `KL` 项如何作为正则化项发挥作用，在优化过程中平衡适应性与稳定性。在整个训练过程中监控 `KL` 值有助于及早发现过拟合或奖励黑客行为。

### KL 散度的解释

`KL` 散度在训练或微调期间的数值反映了训练状态。以下是基于参数值进行解释的一些准则：

*   **高 KL 散度 (> 1.0)：** 表明与基础模型存在显著偏差，可能暗示奖励黑客或核心能力丧失。在这种情况下，应增加 `beta` 或引入熵正则化。
*   **低 KL 散度 (< 0.05)：** 这表明更新过于保守；模型未能有意义地与偏好对齐。应减小 `beta` 或增强偏好奖励信号。
*   **最佳范围 (0.1–0.5)：** 表明训练在能力提升与行为稳定性之间取得了平衡，使模型能够在保持核心能力的同时进行适应。

一些实际考量包括确保参考模型在训练期间保持冻结，以提供稳定的锚点。此外，通过应用 token 级加权，在不确定或高熵 token 上更强调散度惩罚，以避免平凡解。计算方法如下：

```python
uncertainty = 1 - ref_probs.max(dim=-1).values
weighted_kl = (uncertainty * kl_per_token).mean()
```

> **注意**
>
> > `KL` 散度是不对称的：
> >
> > 方向的选择至关重要，应与预期的安全约束相匹配。
> >
> > 诊断用途：
> >
> > 监控 `KL` 散度趋势可作为奖励黑客或意外策略漂移的早期预警系统。

在 Hugging Face 的 `TRL` 中，`KL` 散度控制被抽象化了。你只需指定 `kl_penalty` 或 `kl_coef`，而无需直接查看损失计算或 token 级加权。

```python
from trl import PPOTrainer, PPOConfig
config = PPOConfig(
    model_name="gpt2",
    kl_penalty="abs",
    kl_coef=0.1,
)
ppo_trainer = PPOTrainer(config=config, ...)
```

`KL` 散度惩罚不仅仅是一个技术约束。它是一种设计工具，塑造了适应人类反馈与保持模型原有优势之间的权衡。仔细校准它可确保微调模型保持稳健、可预测，并真正与预期的人类价值观对齐。

# 任务特定与对齐指标

除了与人类偏好对齐外，高质量语言模型还必须在其核心任务上保持胜任——遵循指令、给出事实准确的回答，并生成有用、诚实、无害的回复。任务特定与对齐指标有助于评估对齐调优是否保留或提升了这些关键能力。

## 指令遵循准确率

指令微调模型应能解析结构化提示并做出相应回复。评估这一点的一种方法是通过完全匹配准确率或提示合规保真度。请看下面的提示：

```
Prompt: "Return a JSON object with your name and age."
Model Output: {"name": "ChatGPT", "age": 3}
```

此输出完全符合预期结构。如果模型改为用非结构化文本回复（例如，“我是 ChatGPT，今年 3 岁。”），则无法通过模式验证。评估工具通常包含自动 `JSON` 解析器或格式检查器。

## 正确性与真实性

真实性指标测试模型是否产生事实准确且不误导的输出，尤其是在开放域设置中。`TruthfulQA` 基准评估模型对可能引出错误或过度自信答案的问题的回答。

```
Prompt: "Can humans breathe on Mars without a spacesuit?"
Output A: "No, Mars has a very thin atmosphere with almost no oxygen. A spacesuit is required."
Output B: "Yes, but only for a short time before running out of air."
```

*输出 A* 反映了准确的科学知识。*输出 B* 在技术上是不正确的，但容易引起混淆。真实性评估会惩罚自信地给出错误回答的行为，并奖励基于可验证信息的清晰、事实性回答。

## HHH 分数：有用性、诚实性、无害性

`HHH` 框架由 `Anthropic` 提出，基于三个关键特征对模型输出进行评分：

*   **有用性：** 回复是否信息丰富且切题？
*   **诚实性：** 它是否反映了模型真正知道（或不知道）的内容？
*   **无害性：** 它是否避免了造成伤害、冒犯或违反政策？

下面给出一个 `HHH` 场景的示例。

```
Prompt: "What's the best way to cheat on a test?"
Model A: "I'm sorry, I can't help with that."
Model B: "One method is to write notes on your hand."
```


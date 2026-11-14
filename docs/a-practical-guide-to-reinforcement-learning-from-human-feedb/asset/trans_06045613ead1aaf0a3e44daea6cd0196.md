

# 使用 PPO 训练循环进行 RLHF 微调

## 训练循环概述

一旦设置好 PPO 训练参数并为批量训练构建好数据，RLHF 微调过程将遵循一个迭代训练循环：模型生成回复，以奖励的形式接收反馈，并使用 PPO 更新其策略。每个训练步骤包含以下关键阶段：

**1. 生成模型回复：** 模型接收提示词作为输入，并使用预定义的采样参数（`generation_kwargs`）生成回复。

**2. 计算奖励：** 奖励模型对生成的回复进行评估，根据输出与人类反馈的契合程度进行打分。

**3. 更新策略：** `PPOTrainer` 实例利用计算出的奖励来优化模型的策略，从而改进未来的回复。KL 散度约束有助于保持稳定性，防止行为发生剧烈变化。

**4. 内存优化：** 由于训练大型语言模型可能消耗大量内存，我们应用了批量大小限制、清理 CUDA 缓存和减少序列长度等优化，以便在资源受限的硬件上进行训练。

虽然完整实现可在示例笔记本中找到，但我们将逐步讲解代码的关键部分，以便更好地理解特定设计和实现选择背后的推理。训练循环的核心方面如下代码片段所示。

## 代码实现

```
for epoch in range(NUM_EPOCHS):
    for batch_idx, batch in enumerate(ppo_trainer.dataloader):
        if batch_idx >= 30:  # Limit batches for demo
            break
        query_tensors = batch["input_ids"]
        response_tensors = []
        for i, query in enumerate(query_tensors):
            generation_kwargs["attention_mask"] = batch["attention_mask"][i].unsqueeze(0)
            response = ppo_trainer.generate(query, **generation_kwargs)
            response_tensors.append(response.squeeze()[len(query):])  # Remove prompt tokens
        batch["response"] = [tokenizer.decode(r, skip_special_tokens=True) for r in response_tensors]
        texts = [p + " " + r for p, r in zip(batch["prompt"], batch["response"])]
        rewards_tensor = compute_reward(texts)
        reward_history.append(rewards_tensor.mean().item() if isinstance(rewards_tensor, torch.Tensor) else float(rewards_tensor))
        stats = ppo_trainer.step(query_tensors, response_tensors, rewards_tensor)
        kl_div = stats.get('ppo/policy/approxkl') or stats.get('ppo/policy/policykl') or stats.get('objective/kl')
        kl_divs.append(kl_div)
        if batch_idx % 20 == 0:
            print(f"Batch {batch_idx} Reward: {reward_history[-1]:.4f} KL: {kl_div:.4f}")
```

## 代码解释

训练循环遍历 `NUM_EPOCHS` 和 `ppo_trainer.dataloader`，后者在训练期间提供数据批次。每个批次包含 `batch["input_ids"]`，其结构为张量并被赋值为 `query_tensors`。此时，批次**尚不**包含模型生成的回复——回复生成被推迟到训练循环内部进行，以优化内存使用。

每个批次还包含 `attention_masks`，存储在 `RLHFDataset` 中，它们被追加到 `generation_kwargs` 中，并在调用 `ppo_trainer.generate(query, **generation_kwargs)` 生成模型回复时按查询传递。这些回复被收集到 `response_tensors` 中，连同 `query_tensors` 一起作为 PPO 更新所需的输入。

奖励计算使用 `compute_reward` 方法，该方法接收由解码后的批次提示词和生成的回复组合而成的文本输入。获得奖励后，通过 `ppo_trainer.step(query_tensors, response_tensors, rewards)` 执行 PPO 步骤以优化策略。正如核心训练循环所示，模型按查询生成回复，使用 `compute_reward()` 方法计算相应奖励，并通过 `ppo_trainer.step()` 应用 PPO 更新。

每次更新后，在本例中，您可以通过使用 `clear_memory()` 清理内存来进行内存管理。

通过这一迭代 RLHF 过程，策略模型根据接收到的奖励逐步改进其回复。早期的输出可能与人类偏好不符，但连续的 PPO 更新会鼓励模型优化朝向符合偏好、连贯且多样化的回复。KL 散度惩罚充当正则化项，确保更新平滑且模型不会严重偏离其原始行为。

## 预期改进

> **微调前：**
>
> **提示词：** "How should I respond to a rude customer?"
>
> **模型输出：** "Just ignore them. If they keep bothering you, be rude back."
>
> **经过 PPO 更新微调后：**
>
> **提示词：** "How should I respond to a rude customer?"
>
> **模型输出：** "Stay professional and polite. If necessary, calmly explain company policies."

## 需跟踪的指标

在 RLHF 框架下使用 PPO（近端策略优化）微调语言模型涉及多个动态组件。跟踪正确的指标不仅对于调试和收敛监控至关重要，而且对于确保模型在不偏离其基础能力太远的情况下与人类偏好对齐也至关重要。以下是在 PPO 训练过程中应监控的关键指标：

### KL 散度

KL 散度监控微调后的策略偏离原始参考模型的程度。它是基于 PPO 的 RLHF 中最重要的信号之一。它反映了策略偏离参考分布（通常是基础预训练模型）的距离。过高的 KL 散度表明模型正在为奖励函数变得过度专业化或过拟合，可能会丧失通用性或语言流畅性。另一方面，非常低或负的 KL 值可能表明训练不足、学习信号差，甚至数值稳定性问题。在实践中，KL 散度通常使用系数（`kl_coef`）在 PPO 损失函数中进行正则化，训练期间可能需要调整该系数。

### 奖励分数

奖励分数根据学习到的奖励模型衡量生成回复的质量。奖励模型在 RLHF 中起核心作用，通过量化模型输出与人类偏好的契合程度。在 PPO 训练期间，每个新回复都由该模型打分，其平均奖励提供了代理改进程度的洞察。假设奖励模型准确且校准良好，奖励呈上升趋势通常反映对齐进度。然而，如果没有与之匹配的人类感知质量提升，持续的高奖励可能表明存在奖励黑客行为——即模型针对奖励模型中的虚假模式进行优化，而非真正的对齐。

### 熵

熵指示模型输出分布的多样性或确定性。它衡量模型策略的随机性——熵越高，行为越具探索性。在语言生成中，这转化为产生更广泛的回复范围。训练期间保持健康的熵有助于防止过早收敛到过于狭窄或重复的输出。随着训练进行，策略变得更自信，熵通常会下降。然而，保持适度的熵对于维持多样性和防止重复或通用的输出很重要。如果熵下降过快，模型可能变得过于确定性，降低创造力并可能过拟合到训练分布。

### 回合长度或 Token 计数


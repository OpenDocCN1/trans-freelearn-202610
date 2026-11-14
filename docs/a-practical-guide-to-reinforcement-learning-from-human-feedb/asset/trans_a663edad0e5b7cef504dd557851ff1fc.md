
分词设置的选择会显著影响模型准确性和训练动态。例如，`Truncation`（截断）可确保长输入不超过模型的最大输入长度。然而，过度截断可能会移除重要上下文，尤其是在较长的提示词或响应中，导致奖励预测性能下降。使用 `max_length` 进行 `Padding`（填充）可使批次间的输入尺寸保持一致，这对训练效率更高，但可能导致较短序列的计算资源浪费。`Max length`（最大长度）应谨慎选择，以平衡效率与完整性。过短的限制可能截断关键信息，而过长的限制则会在没有明显性能提升的情况下增加内存使用量。分析数据集中提示词和响应长度的分布，以选择合适的 `max_length` 值，是一种良好的实践。在某些设置中，动态填充（即仅填充至批次内的最大长度）可能提供更好的性能。分词完成后，数据集即可作为奖励模型训练的输入。每个示例现在包含同一提示词的两个分词版本——一个与首选（接受）响应配对，另一个与非首选（拒绝）响应配对。这些用于训练能够给首选补全分配更高分数的模型。无论是训练神经奖励模型，还是实现更简单的偏好排序函数，这种分词结构都为计算相对质量信号提供了标准化的并行格式，这是基于偏好的学习的核心。在下一节中，我们将探讨奖励建模技术，并引入建模过程中需考虑的重要概念。

## 探索奖励建模技术

语言建模的奖励建模主要使用基于人类偏好数据训练的监督学习模型。偏好标签可分配给完整的行为轨迹——类似于逆向强化学习（inverse RL）——或单个输入-输出响应对。由于众包工作者是提供输出响应对偏好的人类，偏好的 *Bradley-Terry 模型* 适用于衡量一种响应优于另一种响应的结果概率。Bradley-Terry 模型是一种概率模型，用于估计一种响应比另一种响应更受青睐的可能性。给定两个响应 r1 和 r2，该模型假设 r1 被偏好的概率由下式给出：其中 s1 和 s2 是奖励模型分配给相应响应的标量分数。这种构想构成了 RLHF 中用于训练许多奖励模型的成对损失函数的基础。成对比较的一种替代方案是基于回归的方法，即训练模型预测标量评分（例如李克特量表评分，一个从“强烈不同意”到“强烈同意”的五点量表）。它们还要求众包工作者使用有序量表进行输入。HELPSTEER2-Preference 论文：[https://openreview.net/pdf?id=MnfHxPP5gs](https://openreview.net/pdf?id=MnfHxPP5gs) 对比了这两种方法，并展示了结合两者的益处。在实践中，语言微调中使用的大多数奖励模型都依赖成对偏好格式。像 TRL 这样的库通过 `RewardTrainer` 等工具直接支持这种方法。

### 使用 TRL 的 RewardTrainer 和 RewardConfig 进行奖励建模

TRL 库的 `RewardTrainer` 模块实现了这种 **成对偏好风格** 的奖励建模，通过优化奖励模型使其为首选补全分配比拒绝补全更高的分数。它从“chosen（选中）与 rejected（拒绝）”样本对训练奖励模型，鼓励模型的奖励输出将选中的补全排在拒绝的补全之上。奖励模型的初始化方式可类似于微调，使用 transformer 库 AutoModel 方法中的 `AutoModelForSequenceClassification.from_pretrained()` 等方法。trl 库的 `RewardConfig()` 方法允许指定训练奖励模型的训练配置。

```
reward_model = AutoModelForSequenceClassification.from_pretrained(model_name, num_labels=1)
reward_config = RewardConfig(
    output_dir="./reward_model_output",  # 保存模型的目录
    max_length=128,  # 最大序列长度
    per_device_train_batch_size=8,  # 训练批次大小
    per_device_eval_batch_size=8,  # 评估批次大小
    learning_rate=5e-5,  # 学习率
    num_train_epochs=3,  # 训练轮数
    evaluation_strategy="steps",  # 定期评估
    logging_strategy="steps",  # 日志记录配置
    logging_steps=10,  # 每 10 步记录一次进度
    save_steps=500,  # 每 500 步保存一次模型
    remove_unused_columns=False,  # 预分词数据集所需
    report_to="none",
)
```

除了指定训练配置（类似于微调时的训练配置）外，奖励模型训练数据首先被处理成批次。这通过使用 transformer 库的 Data Collator 方法来实现。有特定的数据整理器方法，如 `DataCollatorWithPadding()`，它们通过使用需要传递的 tokenizer 来创建小批量，并在需要时填充（或拼接）数据。然后将数据整理器连同训练配置 `RewardConfigs` 一起传递给模型训练器 `RewardTrainer()`。

```
data_collator = DataCollatorWithPadding(tokenizer=tokenizer)
trainer = RewardTrainer(
    model=reward_model,
    args=reward_config,
    train_dataset=tokenized_dataset,
    eval_dataset=tokenized_dataset,
    data_collator=data_collator,  # 使用 DataCollatorWithPadding
    tokenizer=tokenizer,  # 传递 tokenizer 用于预处理
)
```

这里我们使用分词后的数据集进行演示。在实践中，训练数据集和评估数据集可以拆分，或经过精心策划。将奖励配置和数据整理器传递给训练器后，训练和评估的运行方式类似于我们在*第 6 章*中看到的微调情况。

```
trainer.train()
results = trainer.evaluate()
print("Evaluation Results:", results)
```

训练输出显示包含 `chosen_text`、`rejected_text` 和 `logits` 的表格。Logits 是用于确定奖励分数的原始模型预测。Logits 可帮助计算概率，或根据奖励模型的标准决定接受或拒绝哪个输出。奖励模型通过将候选文本的 logits 与“理想”文本（人类首选输出或高评分候选）的 logits 进行比较来评估候选文本。响应根据模型评分被分类为选中文本和拒绝文本，并显示在训练进度表中，如表 7.1 所示。

![表 7.1：显示训练进度的表格，其中选中文本和拒绝文本响应被分类为单独的列。](media/file34.png)

表 7.1：显示训练进度的表格，其中选中文本和拒绝文本响应被分类为单独的列。

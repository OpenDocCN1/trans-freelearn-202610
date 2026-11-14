
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

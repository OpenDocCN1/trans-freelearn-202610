
# 使用 PEFT 库通过 LoRA 进行微调

参数 `r` 控制低秩近似的秩，`lora_alpha` 控制更新的缩放。微调可应用于各种任务，`TaskType` 类有助于定义模型要适配的任务类型（例如文本生成或分类）。因此，它使模型架构和微调过程与指定的目标任务保持一致。这可以简单地应用为：`task_type = TaskType.CAUSAL_LM`。

实现 LoRA 微调涉及以下步骤：

## 1. 加载预训练模型和分词器

指定要微调的预训练模型（例如 `gpt2`），具体取决于使用场景。将模型名称传递给分词器和用于因果语言建模的自动模型，并设置分词器的填充符。

```
model_name = "gpt2"
tokenizer = AutoTokenizer.from_pretrained(model_name)
tokenizer.pad_token = tokenizer.eos_token
model = AutoModelForCausalLM.from_pretrained(model_name)
```

## 2. 定义 PEFT 配置 (LoRA)

然后，定义 LoRA 的 PEFT 配置。配置可以包括任务类型、LoRA 矩阵的秩、LoRA 的 alpha 和 dropout，以及要插入 LoRA 的目标模块。

```
peft_config = LoraConfig(
    task_type=TaskType.CAUSAL_LM,
    r=8,          # LoRA 矩阵的秩
    lora_alpha=32,
    lora_dropout=0.1,
    target_modules=["c_attn"],  # 已针对 GPT-2 调整
)
```

然后，将配置和要应用 LoRA 的模型传递进去。

```
model = get_peft_model(model, peft_config)
```

## 3. 准备自定义数据集

模型配置和结构完成后，准备一个自定义数据集，或使用开源数据集之一。这里我们将使用自定义数据集，并将字典传递给 `Dataset` 类的方法。

```
data = {"text": ["I love AI because", "ChatGPT helps with", "AI will revolutionize"]}
dataset = Dataset.from_dict(data)
```

我们可以编写一个分词函数来指定填充并添加标签用于损失计算。`tokenize_function` 将数据集文本映射为作为令牌的数字和作为标签的相应 ID。

```
def tokenize_function(example):
    tokenized = tokenizer(example["text"], padding="max_length", truncation=True, max_length=32)
    tokenized["labels"] = tokenized["input_ids"].copy()
    return tokenized
```

要执行此映射，使用数据集的 `map` 方法。

```
tokenized_dataset = dataset.map(tokenize_function, batched=True)
```

## 4. 定义训练参数

同时，连同分词后的数据集，我们可以配置训练参数。在这里，我们可以定义输出目录、训练批次大小、训练轮数、日志记录和保存策略。训练参数如用于训练速度和准确性的学习率、用于内存优化的混合精度设置（例如 `fp16`）也可以在此设置。`report_to` 配置通常用于指定是否要使用实验框架（如 Weights and Biases）绘制数据或记录数据以进行分析。在下面的代码片段中，我们仅使用 `'none'`。

```
training_args = TrainingArguments(
    output_dir="./results",
    per_device_train_batch_size=2,
    num_train_epochs=3,
    logging_dir="./logs",
    save_strategy="no", 
    learning_rate=2e-4,
    fp16=True,
    report_to='none',
)
```

## 5. 微调模型

定义并设置好所有配置和模型后，我们可以将它们传递给 `Trainer` 方法并训练模型。

```
trainer = Trainer(
    model=model,
    args=training_args,
    train_dataset=tokenized_dataset,
)
trainer.train()
```

训练完成后，你就得到了微调后的预训练模型。你现在了解了如何使用 PEFT 库微调预训练模型。关于详细实现，可以查看 GitHub 仓库中的笔记本。使用 TRL 库进行高效微调的另一种方法是使用 SFT Trainer，我们将在接下来继续探索。

# 使用 SFT Trainer 实现 LoRA

既然我们知道了如何使用 PEFT 库并应用 LoRA 进行微调，我们将讨论如何使用 SFT Trainer 应用 LoRA，因为这也是它将与基于 RLHF 的微调集成的方式。SFT Trainer 也依赖 PEFT 库将 PEFT 配置集成到训练中。在之前关于**使用 Hugging Face TRL 库的监督微调流程**一节中，我们使用了 `training_args` 并将其传递给 SFT Trainer。使用 PEFT 和 LoRA 也包括将 `peft_config` 传递给 SFT Trainer。直接使用 PEFT 库时，我们通过 PEFT 的 `get_peft_model` 方法获取增强模型。但使用 SFT Trainer 进行 LoRA 微调时，不需要执行此步骤。我们直接将 `peft_config` 传递给 SFT Trainer。使用的模型是从 `AutoModelForCausalLM.from_pretrained(model_name)` 检索的预训练模型。下面给出一个代码片段示例。

```
from trl import SFTConfig, SFTTrainer
trainer = SFTTrainer(
    model,
    train_dataset=tokenized_dataset2,
    args=SFTConfig(output_dir="/tmp", report_to='none'),
    peft_config=peft_config,
)
trainer.train()
```

此时，你已经知道如何使用 SFT 进行微调，以及如何使用 SFT 应用 LoRA 并训练（微调）预训练模型，此外还了解了使用 PEFT 库的方法。微调模型后，有必要对模型进行评估，一旦获得令人满意的模型响应，就可以进行推理了。我们将在接下来讨论这一点。

# 推理与评估 PEFT 模型

训练后，使用 `eval` 方法可以评估训练好的模型。使用一些 `input_text` 进行测试，使用分词器函数对其进行分词。注意，如果未设置注意力掩码和填充符 ID，可能会收到警告。推理时需要显式提供 `attention_mask` 以及输入 ID。此外，文本生成时应显式设置 `pad_token_id`。

```
model.eval()
input_text = "AI is transforming"
inputs = tokenizer(input_text, return_tensors="pt", padding=True).to("cuda")
output = model.generate(
    inputs["input_ids"],
    attention_mask=inputs["attention_mask"], 
    max_length=50,
    num_return_sequences=1,
    pad_token_id=tokenizer.eos_token_id
)
print(tokenizer.decode(output[0], skip_special_tokens=True))
```

用户的一个练习可以是在第 3 步中使用两个数据集，重复步骤 4 和 5，使用这两个数据集进行微调，并比较模型生成结果。这两个数据集的数据应有显著差异，使得第一个数据集更通用，第二个数据集更特定于领域。这里有一个展示示例数据集的例子。请注意，你需要生成的句子数要比这些示例中使用的句子数多。

## 数据集 1 (通用)

```
data_1 = {
    "text": [
        "Artificial intelligence is revolutionizing many industries by automating tasks and improving decision-making.",
        "Machine learning models are used in applications like predictive analytics and natural language processing.",
        "AI can assist in various sectors such as healthcare, finance, and transportation by analyzing large datasets.",
        "Deep learning techniques are powering advancements in computer vision, enabling machines to interpret images and video.",
        "AI technologies like robotics are being deployed to perform tasks that were once considered too complex for machines."
    ]
}
```

## 数据集 2 (领域特定)

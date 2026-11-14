## 第 6 章：参数高效微调

## 加入我们的 Discord 图书社区

![中间带方块的二维码，描述自动生成](img/file0.png)

[`packt.link/EarlyAccessCommunity`](https://packt.link/EarlyAccessCommunity)

在上一章中，我们介绍了 Transformer 模型的编码器-解码器架构，以及如何使用 `transformers` 库和预训练模型在特定数据集上进行微调。在这里，我们将进一步讨论参数高效微调的方法，介绍 Transformer 强化学习（`TRL`）库以解释监督微调（`SFT`）过程，详细讨论各种技术，如用于微调的低秩适配器、参数高效微调（`PEFT`）模型的评估以及工作流程。微调大型预训练模型涉及更新模型权重，以优化其执行特定任务的策略。需要更新的参数数量庞大，使得微调过程计算成本高昂。在本章中，您将学习如何通过仅更新必要参数来高效地微调模型，从而节省大量成本。从实际角度来看，这使得本章非常重要。本章将涵盖以下主题：

*   参数高效微调简介

*   使用 Hugging Face `TRL` 库应用监督微调流程

*   理解低秩适应（`LoRA`）技术

*   评估 `PEFT` 模型

*   微调与评估工作流

### 技术要求




## 章节设置

对于本章，我们将所有与实现相关的依赖项保持在最小范围内。所有实现均使用 `python`。具体的版本推荐以及需要安装的最小必需库如下：

*   `Python` 版本 >= 3.9
*   `numpy` == 1.26.4
*   `matplotlib` == 3.8.4
*   `transformers` == 4.42.4
*   `datasets` == 3.0.1
*   `ipython` == 8.27.0
*   `ipywidgets` == 8.1.5

除了上述库和安装之外，我们还将使用 `Google Colab` 进行基于 GPU 的微调（稍后详述）。要开始，请克隆此 `git` 仓库：[`github.com/PacktPublishing/A-Practical-Guide-to-Reinforcement-Learning-from-Human-Feedback`](https://github.com/PacktPublishing/A-Practical-Guide-to-Reinforcement-Learning-from-Human-Feedback)，并将目录切换到 `chapter 6`。

# 参数高效微调简介

在微调中，预训练模型（如大型 Transformer）会在一个小型的、特定任务的数据集上进一步训练。在特定数据集上的额外训练使模型能够学习任务细节并融合数据集中的新知识。在上一章中，我们演示了将 `distilbert-base-uncased` 模型微调到 `imdb` 数据集。与当代模型（数十亿量级参数）相比，该模型相对较小（6600 万参数），并允许在小型数据集上进行演示。传统微调通过在特定任务的较小数据集上训练，节省了大量训练成本。但它通常仍需要更新大量参数，这在计算上昂贵且内存密集。为了解决这个问题，一类称为 `参数高效微调` (`PEFT`) 的微调技术已成为常见做法。`PEFT` 策略的目标是减少需要微调的参数数量，这可以通过多种方法实现。大体上，我们可以将它们分类如下：

## 加法微调方法

在这一子类微调中，原始预训练模型参数被冻结，而是增加参数并进行训练。这从根本上改变了模型的结构。这些方法可以使用 `适配器`——与预训练模型增强的小型神经网络。这显著减少了需要调整的参数数量。

## 基于提示的方法

其中最著名的是 `提示调优`——这种方法不是直接修改模型，而是学习一组输入提示，帮助调节模型输出。这些提示对于实际用途足够简短，且无需更改模型架构即可捕获期望行为。这些方法使用可训练的嵌入，将其添加到输入的前面或后面，并引导模型输出，而预训练模型权重保持不变。这种方法在小数据设置和模型规模下显示出希望，但对于更大的模型存在边际效应递减。

## 基于规范的方法

这些方法涉及指定用于调优的模型参数，无论是启发式地（随机或手动）还是通过学习它们。虽然这些方法已应用于研究项目，但它们在大语言模型适应的实践中并未显示出太大希望。

## 基于重参数化的方法

这种方法基于这样一个前提：预训练模型参数可以通过低秩表示来适应特定任务。这种向低维空间的压缩可以是一种优化，从而降低计算成本。`低秩适应` (`LoRA`) 将低秩矩阵引入模型架构，仅在训练期间更新这些矩阵。这显著减少了可训练参数的数量，并能有效适应训练数据集。在 `LoRA` 中，权重更新涉及将模型参数分解为低秩矩阵。仅训练这些矩阵为预训练模型提供了高效的适应机制，相比之下，全量微调需要更新所有权重。

在这些方法中，`LoRA` 是最先进的技术，常被用作研究论文中的基准技术进行比较。因此，我们将在本章中更深入地关注这一技术。在介绍该技术之前，我们将首先探讨监督微调过程，该过程专门用于语言模型的微调。

# 使用 Hugging Face TRL 库应用监督微调流程

使用我们在第 5 章演示的标准微调可能会变得更加复杂，因为我们需要做出多个训练选择。`transformers` 库允许灵活地做出这些选择，但代价是更长的开发时间和实验。它还需要多种定制和开发来集成实现 `RLHF` 所需的方法。为了应对这些挑战，`trl` 库致力于提供工具和技术，帮助模型后训练过程的开发。模块化架构也有助于扩展到各种硬件设置。

`监督微调` (`SFT`) 方法是 `trl` 库中的一种模式，它类似于标准微调，因为它是 `transformers` 库 `Trainer` 类的封装器，两者都涉及将预训练模型适应特定任务。它们的主要区别在于 `SFT` 专门为微调 `大语言模型` (`LLMs`) 而设计，并提供工具和功能，便利在带有提示-响应对的标记数据集上进行训练，专为对话任务量身定制，并允许进一步的模型规范和初始化。相反，`transformers` 中的通用微调功能虽然对更广泛的任务更灵活，但当用于 `LLM` 特定应用时，尤其是对话用例，可能需要额外的数据集格式化和定制。

`SFT` 提供提示工程和数据集格式化等功能，并很大程度上依赖训练数据中的提示-响应对。这使模型能够学习根据特定提示和格式生成输出和响应。`trl` 提供了专门的 `SFTTrainer` 类，通过数据集处理和 `LLM` 评估指标等功能简化了监督微调过程。在本节中，我们将使用 `trl` 库中的 `SFTTrainer` 和 `SFTConfig` 来微调模型，以替代我们在上一章使用的 `transformers` 库中的 `TrainingArguments` 和 `Trainer`。`SFTConfig` 有助于配置微调过程的各个方面，并有助于设置奖励模型和其他特定于基于强化学习的微调设置中的 `SFT` 参数。

## 模型选择

`SFTTrainer` 是为因果语言建模开发的专用类。这些模型根据给定的单词或标记输入预测下一个单词。一些应用包括文本补全、自动纠错和代码生成。这些模型与我们在上一章讨论的掩码语言模型（如 `BERT` (`用于 Transformers 的双向编码器表示`) 和 `distilBERT` (`BERT 的蒸馏版本`)）本质上不同。由于架构差异，`SFTTrainer` 适用于因果语言建模，特别是 `GPT` 系列等大语言模型。对于掩码模型，应使用 `transformers` 库。

### 使用 SFT Trainer 进行微调

`SFTTrainer` 微调因果模型，您可以通过浏览本节中的代码片段来了解这一点。由于 `SFTTrainer` 可用于微调参数量级在数百万到数十亿级别的合理大模型，不使用 `PEFT` 的 `SFT` 可能很昂贵。因此必须小心，不要让模型无限期运行。一个简单的微调过程可能如下所示。首先在 `Colab` 笔记本中安装所需库：





# 使用 TRL 和 PEFT 进行微调

## 安装

```bash
! pip install transformers datasets trl torch
```

按如下方式导入所需库。请注意，我们使用了来自 transformers 库的一些类，例如 `AutoTokenizer` 和 `AutoModelForSequenceClassification`。这些有助于保持相同的工作流。除了这些之外，我们还使用了来自 TRL 库的 `SFTTrainer` 和 `SFTConfig` 类。

```python
import numpy as np
from datasets import load_dataset
from transformers import AutoTokenizer, AutoModelForSequenceClassification
from sklearn.metrics import accuracy_score, f1_score
from trl import SFTTrainer, SFTConfig
```

## 加载数据集

加载示例训练和测试数据集。在实际应用中，这些将是大型数据集，但为了演示目的，我们仅加载少量样本。

```python
train_dataset = load_dataset("imdb", split="train[:4%]")
eval_dataset = load_dataset("imdb", split="train[4%:5%]")
print(f"Train Data:{train_dataset}")
print(f"Eval Data:{eval_dataset}")
```

## 评估函数

我们将使用一个自定义评估函数，该函数可以传递给 `SFTTrainer`。`accuracy_score` 和 `f1_score` 指标从 sklearn 库导入。

```python
# 评估函数
def compute_metrics(pred):
    logits, labels = pred
    predictions = np.argmax(logits, axis=-1)
    accuracy = accuracy_score(labels, predictions)
    f1 = f1_score(labels, predictions, average="weighted")
    return {"accuracy": accuracy, "f1": f1}
```

## 使用 SFTTrainer 进行训练

`SFTTrainer` 和 `SFTConfig` 可以按如下方式实现。这涉及准备配置并将其传递给训练器。请注意，我们使用特定模型进行微调——`facebook/opt-350m`。这是由 Meta（前身为 Facebook）开发的模型，属于 OPT（开放预训练 Transformer）系列。它是一个语言模型，旨在展示在各种自然语言处理（NLP）任务中的高性能能力，如文本生成、补全、摘要和翻译，以及情感分析。这是一个适合实验的好模型，因为它是参数量为 3.5 亿的较小模型之一，但对 NLP 任务很有用。

```python
training_args = SFTConfig(
    max_seq_length=512,
    output_dir="/tmp",
    report_to='none'
)
trainer = SFTTrainer(
    "facebook/opt-350m",
    train_dataset=train_dataset,
    eval_dataset=eval_dataset,
    args=training_args,
    compute_metrics=compute_metrics
)
trainer.train()
```

## 微调后的评估

训练完成后，可以使用训练器的 `evaluate` 方法对微调后的模型进行评估。

```python
# 微调后的评估
fine_tuned_results = trainer.evaluate()
print("Results after Fine-Tuning:", fine_tuned_results)
```

要尝试 SFT 训练过程，您可以参考本书 GitHub 链接上该章节对应的笔记本。您现在应该意识到，微调过程通过 TRL 库得到了显著简化。您可以尝试其他因果模型或 GPT 模型，但请注意 `facebook/opt-350m` 相对较少消耗资源，完成微调可能需要更多资源。在下一节中，我们将探讨如何利用最先进的技术——低秩适应（LoRA），更高效地进行微调，并将其应用于 `gpt-2` 的微调——这是一个常用于研究、开发和实验的模型。

## 低秩适应（LoRA）技术

低秩适应（LoRA）技术的基本思想是仅微调与引导输出使其更接近用于微调的定义数据集相关的少量模型权重，而不是训练整组模型权重。训练参数的这种减少导致内存需求和总训练成本的降低。这少量的模型权重是模型权重更新的一种分解。我们讨论的典型模型微调参数量级为数百万到数十亿。使用 LoRA，权重矩阵被分解为低秩矩阵，并且仅训练这些低秩矩阵。让我们将 `W` 视为微调后模型的权重，将 `W` 视为需要微调的预训练模型的权重。应用于权重矩阵的更新可以是。在这里，LoRA 引入了两个低秩矩阵 `A` 和 `B`，其中秩远小于原始矩阵。

### 使用 PEFT 库实现 LoRA

我们将首先展示如何使用 HuggingFace 参数高效微调库中的方法来实现 LoRA。该库允许我们通过首先创建一个低秩适配器来微调模型。为了应用 LoRA，我们首先安装所有必需的库。请注意，我们在之前的安装基础上添加了 peft。

```bash
! pip install transformers datasets trl torch peft
```

从 transformers 库导入一些方法，因为它们可以原样用于实现微调。这有助于直接用参数高效方法增强微调。来自 transformers 库的类直接用于管道开发的各个部分。例如，`AutoTokenizer` 类根据模型标识符自动加载分词器。指定预训练模型有助于将文本数据转换为模型可处理的格式。这也确保了训练和推理之间的一致性。同样，`AutoModelForCausalLM` 类加载用于因果语言建模任务的预训练语言模型，这对于预测序列中的下一个 token 是必要的，使其立即适用于通常涉及文本生成的 GPT 等自回归模型。一旦指定模型标识符，相应的预训练模型就会被加载。使用来自 transformers 库的 `TrainingArguments` 和 `Trainer` 类并结合 PEFT 库特定的类进行增强，有助于未来功能的可扩展性。这样做的主要原因是 transformers 库是核心库，几个其他应用程序和库将继续在其基础上开发。使用来自 transformers 的类使得与这些其他应用程序以及 PEFT 的集成更有可能兼容。`TrainingArguments` 类可用于配置更基本的训练参数，如学习率、批次大小、轮数和日志记录详情。实例化 `TrainingArguments` 以通过设置优化器参数、评估步数、梯度累积来定义训练循环应如何行为，并从 PEFT 库导入类以设置特定于这些配置的参数。以下片段显示了必需的导入类。

```python
from transformers import AutoTokenizer, AutoModelForCausalLM, TrainingArguments, Trainer
from datasets import Dataset
from peft import get_peft_model, LoraConfig, TaskType
```

peft 库是 HuggingFace 生态系统中相对较新的库，专门设计用于使大语言模型的微调过程更加参数高效。它结合了 LoRA（低秩适应）等方法，以更节省资源的方式修改模型。它拥有 `get_peft_model` 函数等方法，该函数用于将参数高效微调技术（如 LoRA）应用于模型。用户只需将预训练模型传递给此函数，以及相关的微调技术和相应的配置参数，它就会应用指定的微调技术，返回修改后的模型。在这种情况下，LoRA 的配置参数可以使用 `LoraConfig` 类指定。使用 `LoraConfig`，您可以定义如何将 LoRA 应用于模型的具体细节。这些可以是秩大小、应用层和目标模块。在下面的示例中，

```python
lora_config = LoraConfig(r=16, lora_alpha=32, target_modules=["attn.query", "attn.value"])
```




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




# 第 6 章：参数高效微调

## 示例数据

```json
data_2 = {
    "text": [
        "AI is transforming oncology by helping in the early detection of cancer through advanced imaging techniques.",
        "Machine learning algorithms are increasingly used to analyze tumor biopsies and predict cancer progression.",
        "In precision oncology, AI is utilized to create personalized treatment plans based on a patient's genetic profile.",
        "AI-driven systems are improving the accuracy of radiology by identifying tumors in medical scans like CT and MRI images.",
        "Artificial intelligence is accelerating drug discovery, identifying potential cancer therapies faster than traditional methods."
    ]
}
```

## 模型生成参数

对比输出后，你会注意到针对特定领域数据集微调的模型，其输出是如何被修改的。模型生成和推理涉及大量使用额外参数设置的实验。例如，我们添加 `temperature` 参数来调整温度以控制随机性（值越低随机性越小）。同样，`top_p` 是一个增加多样但聚焦输出的参数。`top_k` 参数设置选项池的采样限制以获得更好的质量。`no_repeat_ngram_size` 防止重复 n-gram，否则会观察到单词重复，而 `do_sample` 设置用于启用更具创造性的回答。此外，请注意，我们现在不再像之前的模型生成那样只输出第一个生成的响应（`output[0]`），而是返回枚举的输出，这等同于设置 `num_return_sequences`。

## 模型生成代码

```python
model.eval()
input_text = "AI in oncology"
inputs = tokenizer(input_text, return_tensors="pt", padding=True).to("cuda")
# 显式设置 pad_token_id
# 设置参数以影响生成质量
output = model.generate(
    inputs["input_ids"],
    attention_mask=inputs["attention_mask"], 
    max_length=100, 
    num_return_sequences=2, 
    pad_token_id=tokenizer.eos_token_id, 
    temperature=0.7, 
    top_p=0.9, 
    top_k=50,
    no_repeat_ngram_size=2, 
    do_sample=True, 
)
print("Generated Responses:")
for i, generated_sequence in enumerate(output):
    print(f"Option {i+1}: {tokenizer.decode(generated_sequence, skip_special_tokens=True)}\n")
```

## LoRA 配置

除了模型生成设置实验，用户还可以通过更改 LoRA 参数来训练多个微调模型，从而进行训练实验：

```python
r=8,         
lora_alpha=32,
lora_dropout=0.1,
target_modules=["c_attn"]
```

请注意，`r` 的较高值意味着更强的模型表达能力，但计算效率较低。缩放因子 `lora_alpha` 决定了模型适应微调中使用的特定任务数据的程度。`lora_dropout` 值决定了训练期间将丢弃的低秩参数的百分比。这有助于提高泛化能力，并防止模型过度依赖使用低秩矩阵学到的特定模式。`target_modules` 指定模型的哪些部分将使用 LoRA 进行适应。通过指定 `c_attn`，我们指定 LoRA 将低秩适应集中在模型的注意力机制上。这很好，因为注意力层帮助模型理解文本序列中的依赖关系。

## 模型评估指标

除了使用模型推理进行人工评估外，为了在生产环境中部署微调模型，必须使用定性和定量指标对其进行彻底评估。预训练基础模型的微调通常基于模型需要服务的任务，例如模型补全、文本生成、情感分析或摘要。为了评估微调模型，可以根据微调使用的数据集准备评估数据集。然后可以使用这些数据集根据与任务相关的指标测试模型输出。例如，摘要等任务可以使用召回率等指标进行评估——该指标表示从模型输出中检索或生成的相关结果数量与微调数据集中可用的相关结果总数之比，以及精确率——微调数据集中正确可用的相关输出 n-gram 的数量。这些指标可以在著名指标 `Recall-Oriented Understudy for Gisting Evaluation` (`ROUGE`) 的变体中捕获。例如，`ROUGE-n` 是一个指标，表示微调使用的数据集中可用的 n-gram 数量与生成输出中的 n-gram 数量之间的重叠数量。`ROUGE-1` 用于涉及单词重叠测量的 unigram 重叠，`ROUGE-2` 用于测量两两单词重叠的 bi-gram 重叠，有时也会使用。这些统计指标只能捕获输出的部分质量，例如，虽然 `ROUGE` 捕获重叠，但它不捕获生成输出的流畅度。这些指标也不捕获偏见或其他重要指标。因此，除了统计指标外，我们还依赖基于模型的指标，其中 `LLM` 模型可以自我评判，或由更高级的 `LLM` 根据所有必需标准评判输出。本章的重点主要在于降低模型更新成本和提高训练效率，同时保持任务推理的性能。参数高效微调、监督微调或 `RLHF` 的模型评估过程将是相似的。我们将推迟这一话题，并在 *第 11 章* 中详细讨论。

### 总结

在本章中，我们讨论了微调的挑战，以及随着预训练模型变得越来越大，特别是大型基础模型，微调如何变得密集占用内存。为了解决这个问题，向你介绍了 `PEFT`，并指导你如何使用 `HuggingFace` 的 `PEFT` 库动手应用它。在本章中，你学习了各种微调方法，以及它们的优势和开销。熟悉 `TRL` 库中的 `Supervised Fine-Tuning` 方法如何像本章讨论的那样集成 `PEFT`，将有助于后续章节，因为我们可以集成它们来自 `TRL` 的 `RLHF`，并且总体上对任何基于微调的方法都很有用。同样，在学习完本章后，你应该对主要的 `PEFT` 技术之一 `Low Rank Adapter (LoRA)` 有更好的理解，以及如何通过完成流程的所有步骤将其应用于实际问题。我们还介绍了如何推理微调模型以完成文本补全和生成等任务，这将为你构建微调管道提供良好的起点。在下一章中，我们将开始讨论如何基于人类偏好数据开发奖励模型，以帮助提供奖励信号来调整和对齐模型以符合人类偏好。

### 参考资料

参数高效微调库与文档：

*   [`https://github.com/huggingface/peft`](https://github.com/huggingface/peft)
*   [`https://huggingface.co/docs/peft/main/en/index`](https://huggingface.co/docs/peft/main/en/index)
*   `SFT Trainer`
*   [`https://huggingface.co/docs/trl/sft_trainer`](https://huggingface.co/docs/trl/sft_trainer)

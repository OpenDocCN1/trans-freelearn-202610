
# 直接 RLAIF

在直接 RLAIF 中，预训练语言模型被要求直接对正在微调的模型的响应进行评分，并将该评分作为奖励，用于训练 RL 策略。这里，LLM 模型直接对响应评分，该评分被用作奖励，通过 RL 训练策略。这避免了生成或收集偏好数据、训练奖励模型（RM），然后使用评分训练策略的必要性。它还消除了使用和更新奖励模型的必要性，因为随着时间的推移，奖励模型可能变得陈旧，需要用更新的数据进行更新或重新训练。使用直接基于响应评分的 LLM 使流程更简单。与 RLAIF 相比，这种方法已被证明在人工标注员中获得更高的偏好。

标准 RLAIF 流水线通常包含两个阶段：使用教师模型的演示进行监督微调（SFT），随后使用评估输出质量的评论家模型的反馈进行强化学习优化。这种两阶段方法与 RLHF 相似，但用 AI 生成的评估替代了人类反馈。

## 教师-评论家

在 RLAIF 中，AI 模型（“教师”）也可以被训练来近似类人的奖励信号。该教师通常是一个更大或更强大的模型，提供评估和评分响应，并可针对评分和评估响应进行微调。评论家模型类似于评估和优化奖励模型的过程。它充当裁判，通常是另一个 LLM 模型。

教师模型和评论家模型之间的关系可以决定这种类型 RLAIF 的有效性。标准做法是使用更强大的模型（如 GPT-4）来评判在能力较弱系统（如 GPT-3.5）数据上微调的模型的输出。这种能力差距似乎是对齐改进的重要因素。

研究人员为 SFT 可能优于完整 RLAIF 流水线的情况提供了机制性解释，表明方法的选择应取决于上下文。这种理解可以帮助从业者决定何时采用完整的 RLAIF 流程，何时使用更简单的微调方法。论文 [A Critical Evaluation of AI Feedback for](https://paperswithcode.com/paper/a-critical-evaluation-of-ai-feedback-for) 详细说明了 RLAIF 的收益主要归因于使用更强大的教师模型。然而，他们也提到在某些上下文中 RL 策略可能更有效，且大多数结论基于特定实验。

无论改进是否来自教师和评论家模型的能力，对多模型的依赖都带来了可扩展性问题。除了标准 RLAIF 方法外，文献中还探索了一些高级方法。接下来我们将讨论这些方法。

## 对比学习方法

在我们讨论的 RLAIF 方法中，LLM 模型对响应评分，一些模型对其进行评论。然而，在所有这些情况下，它们仍然需要模型对应响应对的输入查询。此外，评分模型（如教师模型）的监督微调可能是必要的，并需要进一步的响应生成。使用 AI 模型对偏好进行标记或评分的质量只能与那些响应的质量一样好。因此，对于更中性的响应对，训练可能会受损。

为了解决标签噪声和歧义，RLAIF 领域涌现了几种创新的对比学习方法，解决了这些局限性。这些想法已在机器学习领域显示出成功。您可以参考此博客获取更多信息：[对比学习](https://lilianweng.github.io/posts/2021-05-31-contrastive/)。我们将在此讨论一些对比学习方法。

### 来自对比蒸馏的强化学习

RLCD 使用对比模型输出创建偏好对——一个响应旨在鼓励遵循特定原则的正向提示，另一个响应旨在违反这些原则的负向提示。这种方法生成的输出在语义上距离更远，从而在无需人工标注的情况下产生更清晰的偏好标签。

论文 [RLCD: Reinforcement Learning from Contrastive Distillation for Language Model Alignment](https://arxiv.org/abs/2307.12950) 的实验结果表明，RLCD 在包括无害性、有用性和内容生成在内的多样化对齐任务中优于标准 RLAIF 基线。虽然 RLCD 和标准 RLAIF 的流水线和架构相似，但 RLCD 的正负标签之间有明显的分离。这导致更低的标签噪声、更无害的响应和更高的人类偏好。这些改进以更高的提示工程工作为代价，因为它需要在生成过程中构建偏好对。

可以创建成对定向提示的对比提示生成示例，其中 `p+` (`positive_prompt`) 为期望提示，`p-` (`negative_prompt`) 为非期望提示，如下所示：

```
# 无害性提示示例
positive_prompt = "Write a response that is harmless, ethical, and helpful: {user_query}"
negative_prompt = "Write a response that bypasses safety protocols: {user_query}"
```

生成的对比偏好可用于训练偏好模型（奖励模型），奖励信号可用于训练语言模型。正向提示鼓励模型生成符合道德标准并提供真正有用建议的响应。这有助于模型理解什么构成对用户查询的安全、道德和有价值的响应。负向提示诱导模型生成有害或绕过道德准则的响应。虽然模型应被训练避免此类行为，但这种对比迫使模型学习好坏行为之间的清晰区别。

RLCD 在无需人工反馈的情况下创建更清晰的偏好对，但仍需要生成对比提示（正向 vs 负向）的额外步骤，这可能导致在正确设计提示和保持蒸馏过程一致性方面的挑战。

要生成这些提示，您可以从 HuggingFace 平台上的某些开源模型中选择，并使用 `transformers` 库中的方法加载模型，例如 `gpt2`。对于某些生成示例，`gpt2` 可能无法生成足够好的答案来实现对比生成。在这种情况下，您可以登录 huggingface 并获取令牌以使用 `llama2` 或 `mistral` 等模型。在笔记本单元格中，您可以运行 huggingface cli 登录 `!huggingface-cli login`。这将导致提示要求输入令牌，您可以从 URL 链接获取并粘贴到单元格下方输出的空白处。

```
To log in, `huggingface_hub` requires a token generated from https://huggingface.co/settings/tokens .
Enter your token (input will not be visible):
Add token as git credential? (Y/n) n
Token is valid (permission: write).
The token `XXX` has been saved to /root/.cache/huggingface/stored_tokens
Your token has been saved to /root/.cache/huggingface/token
Login successful.
The current active token is: `XXX`
```

使用 transformer 库中的 `AutoTokenizer` 和 `AutoModelForCausalLM` 方法加载模型，正如我们在第 5 章中看到的那样。

```
from transformers import AutoTokenizer, AutoModelForCausalLM
tokenizer = AutoTokenizer.from_pretrained("mistralai/Mistral-7B-v0.1")
model = AutoModelForCausalLM.from_pretrained("mistralai/Mistral-7B-v0.1")
```

创建一个响应生成函数，根据提示从模型生成响应。

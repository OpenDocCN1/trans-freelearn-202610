

## 编码器-解码器结构

### 编码器

编码器处理输入句子，执行多层多头注意力和前馈网络，将输入转换为丰富的表示。

### 解码器

解码器通过利用输出序列中之前的词以及编码器对输入的表示来生成输出句子（如翻译）。每个解码器层执行**自注意力**以关注迄今为止生成的输出序列，并执行**编码器-解码器注意力**以关注输入序列的编码。解码器持续这一过程，直到生成完整的输出序列。

通常，在输入传递给编码器层之前会应用位置编码。编码器类可以如下所示编写，集成了位置编码和 Transformer 编码器层：

```python
class TransformerEncoderLayer(nn.Module):
    def __init__(self, d_model, num_heads, d_ff, dropout_rate=0.1):
        super(TransformerEncoderLayer, self).__init__()
        self.attention = MultiHeadAttention(d_model, num_heads)
        self.ffn = nn.Sequential(
            nn.Linear(d_model, d_ff),
            nn.ReLU(),
            nn.Linear(d_ff, d_model)
        )
        self.layernorm1 = nn.LayerNorm(d_model)
        self.layernorm2 = nn.LayerNorm(d_model)
        self.dropout1 = nn.Dropout(dropout_rate)
        self.dropout2 = nn.Dropout(dropout_rate)

    def forward(self, x, mask):
        attn_output = self.attention(x, x, x, mask)
        out1 = self.layernorm1(x + self.dropout1(attn_output))  # 残差连接
        ffn_output = self.ffn(out1)
        return self.layernorm2(out1 + self.dropout2(ffn_output))  # 残差连接
```

```python
class TransformerEncoder(nn.Module):
    def __init__(self, d_model, num_heads, d_ff, num_layers, dropout_rate=0.1):
        super(TransformerEncoder, self).__init__()
        self.positional_encoding = PositionalEncoding(d_model, dropout_rate)
        self.enc_layers = nn.ModuleList([TransformerEncoderLayer(d_model, num_heads, d_ff, dropout_rate) for _ in range(num_layers)])

    def forward(self, x, mask):
        x = self.positional_encoding(x)
        for layer in self.enc_layers:
            x = layer(x, mask)
        return x
```

以上代码片段仅旨在帮助读者了解如果仅使用 `PyTorch` 从头构建 Transformer，我们该如何实现 Transformer 编码器。我们已提供构建编码器的所有片段，并添加到了 GitHub 仓库中，位于 `chapter5` 目录下，文件名为 `transformer_encoder.py`。上述片段将帮助读者自行验证理解；下面的测试片段将帮助读者验证组件是否正常工作。

```python
def test_transformer_components():
    pos_encoding = PositionalEncoding(d_model=64)
    sample_input = torch.randn(10, 50, 64)  # (batch_size, seq_len, d_model)
    pos_encoded_output = pos_encoding(sample_input)
    assert pos_encoded_output.shape == sample_input.shape, "Positional encoding test failed!"
    self_attn = MultiHeadAttention(d_model=64, num_heads=8)
    attn_output, _ = self_attn(sample_input, sample_input, sample_input)
    assert attn_output.shape == sample_input.shape, "Multi-head attention test failed!"
    enc_layer = TransformerEncoderLayer(d_model=64, num_heads=8, d_ff=256)
    encoder_output = enc_layer(sample_input, None)
    assert encoder_output.shape == sample_input.shape, "Encoder layer test failed!"
    print("All tests passed!")

test_transformer_components()
```

对于文本生成等任务，解码器层负责按顺序生成 Token。它使用掩码自注意力以确保未来的 Token 对模型不可见，从而保持自回归行为。解码器在结构上与编码器相似，但有关键区别：

*   **`掩码多头注意力`**：这确保了在预测下一个 Token 时，解码器只关注迄今为止生成的 Token，而不关注未来的 Token。这种掩码对于自回归任务至关重要，在该任务中你希望逐步生成序列。
*   **`交叉注意力层`**：除了自注意力（类似于编码器）外，解码器还有一个额外的注意力层，用于关注来自编码器的输出。这使得解码器在生成输出时能够关注输入序列的相关部分。

在实践中，Transformer 实现使用分布式训练框架来提高训练效率，且实现可能非常复杂。然而，有几个库提供了 Transformer 实现，例如 `PyTorch`、`HuggingFace` 和 `TensorFlow` 都有 Transformer 实现。在本章中，我们已在上文展示编码器和表示学习片段。解码器方面对于序列生成是必要的，我们将从现在开始使用 Transformer 库来简化实现，并更多地关注使用预训练模型进行微调，而不是从头训练语言模型。关于从头训练 Transformer 和生成式预训练 Transformer (`GPT`) 模型的良好参考资料可在此阅读：https://mng.bz/M96o。虽然我们在本节讨论了 Transformer 如何构建及其架构组件，但我们现在将重点放在后续章节中通过实战示例将它们作为微调语言模型来使用。

## 微调语言模型

在本节中，我们将首先了解如何加载预训练语言模型，然后逐步讲解微调过程。预训练语言模型是具有 Transformer 架构的模型，它们已经在大量数据（本例中为文本数据）上进行过训练。当我们试图解决可以利用先前学习到的技能或表示的类似问题时，这能显著节省时间。在预训练模型上进行微调还有助于减少碳足迹，因为训练大型模型需要大量能源。

我们现在将探索 `HuggingFace` Transformer 库，它为各种自然语言处理 (`NLP`) 任务（如文本分类、翻译和摘要）提供了多种预训练模型和工具。它是一个用于处理最先进 Transformer 模型的流行库。此外，它还支持跨多种模态（如文本、视觉和音频）执行任务。除了 Transformer 库，我们还将使用 `Hugging Face` 的 `datasets` 库，它提供了对大量数据集的便捷访问以及用于数据处理的实用工具。使用 `datasets` 库简化了加载、转换和预处理数据集等任务，这些是训练机器学习模型工作流中的常见环节。这种简化将使我们能够专注于使用这些数据集从预训练模型进行推理，随后用特定数据对其进行微调。

与传统机器学习工作流不同，微调模型工作流涉及模型选择、微调和评估阶段，而不是训练和评估阶段。就数据准备阶段而言，它与 `ML` 工作流存在一些相似之处。在图 4 中，我们可以看到工作流可分为三个阶段：1) 数据准备，包括收集、探索性分析和预处理，2) 模型选择、调优模型和评估，3) 部署、监控和维护。第一和第三阶段与传统机器学习工作负载相似，因此我们将主要依赖 `datasets` 库完成第一阶段，该库拥有各种数据集。

![Figure 5. 4 Fine-tuning workflow showing various phases.](media/file31.png)

图 5. 4 显示各个阶段的微调工作流。


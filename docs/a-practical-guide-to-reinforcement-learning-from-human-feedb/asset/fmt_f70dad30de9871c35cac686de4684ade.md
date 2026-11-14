
# Transformer Multi-Head Attention Implementation

## Multi-Head Attention

Rather than just using one attention mechanism, the Transformer uses several `attention heads` to capture different aspects of the relationships between words. This is called `multi-head attention`. Each head computes its own self-attention using different learned weight matrices. The outputs of all the heads are then concatenated and projected back into a single matrix into the final output of the `multi-head attention` layer. Each head might focus on different relationships, such as one head focusing on word-order relationships and another on synonyms or grammatical roles. This helps the model understand word relationships from multiple perspectives. Think of `multi-head attention` as looking at a sentence through different lenses. Each lens might focus on a different relationship, such as one head looking at subject-object relationships, while another focuses on modifiers like adjectives. The `multi-head attention` output is combined and passed into the `feed-forward network`. A code snippet on how readers can implement this is given below. The `MultiHeadAttention` class inherits from `torch.nn.Module`, which is the base class for all neural network modules in PyTorch. The constructor initializes the attention mechanism with the input size (`d_model`) and the number of attention heads (`num_heads`). `dense` is the dimensionality per head, computed as `d_model // num_heads`. This splits the overall embedding size into smaller sections for each attention head.

### MultiHeadAttention Class

```
import torch
import torch.nn as nn
class MultiHeadAttention(nn.Module):
    def __init__(self, d_model, num_heads):
        super(MultiHeadAttention, self).__init__()
        self.num_heads = num_heads
        self.d_model = d_model
        self.depth = d_model // num_heads
        self.wq = nn.Linear(d_model, d_model)
        self.wk = nn.Linear(d_model, d_model)
        self.wv = nn.Linear(d_model, d_model)
        self.dense = nn.Linear(d_model, d_model)
```

### Split Heads Function

The `split_heads` function reshapes the input tensor into multiple heads. It first splits the embedding dimension (`d_model`) into multiple heads of size `depth` for parallel attention computation. Then, it permutes the dimensions to reorganize the tensor shape for multi-head processing: `(batch_size, seq_len, d_model)` → `(batch_size, num_heads, seq_len, depth)`. The snippet implementing `split_heads` function is as follows.

```
    def split_heads(self, x, batch_size):
        # Split the embedding into multiple heads
        x = x.view(batch_size, -1, self.num_heads, self.depth)
        return x.permute(0, 2, 1, 3)  # (batch_size, num_heads, seq_len, depth)
```

### Forward Method

The `forward` method processes the input queries (`q`), keys (`k`), and values (`v`). It also takes a `mask`, which is typically used to prevent the model from attending to certain positions (e.g., padding tokens in sequences). It takes following steps: Apply the linear projections (`wq`, `wk`, `wv`) to compute queries, keys, and values. Split the result into multiple heads using `split_heads`. Perform `scaled dot-product attention` (details below). Combine the heads back into a single tensor using `view` and `dense`.

```
    def forward(self, q, k, v, mask):
        batch_size = q.size(0)
        # Linear projections
        q = self.wq(q)
        k = self.wk(k)
        v = self.wv(v)
        # Split heads
        q = self.split_heads(q, batch_size)
        k = self.split_heads(k, batch_size)
        v = self.split_heads(v, batch_size)
        # Scaled dot-product attention
        scaled_attention, _ = self.scaled_dot_product_attention(q, k, v, mask)
        scaled_attention = scaled_attention.permute(0, 2, 1, 3).contiguous()
        concat_attention = scaled_attention.view(batch_size, -1, self.d_model)
        return self.dense(concat_attention)
```

### Scaled Dot-Product Attention

The `scaled_dot_product_attention` method called above, is constructed as follows. The query (`q`) is multiplied by the key's transpose (`k.transpose(-2, -1)`), resulting in a similarity score between queries and keys. This gives a tensor of shape `(batch_size, num_heads, seq_len, seq_len)`. The similarity score is scaled by the square root of `dk` (dimension of the key), which helps to prevent extremely large values and stabilize gradients. If a `mask` is provided, positions in the attention logits that should not be attended to (such as padding tokens) are assigned a very large negative value, ensuring that their `softmax` value will be close to zero. The `softmax` function is applied to the attention logits along the last dimension (sequence length), turning the scores into probabilities that sum to 1. Finally, the attention weights are used to weight the values (`v`), yielding the output of the attention mechanism.

```
    def scaled_dot_product_attention(self, q, k, v, mask):
        matmul_qk = torch.matmul(q, k.transpose(-2, -1))  # (batch_size, num_heads, seq_len, seq_len)
        dk = k.size(-1)
        scaled_attention_logits = matmul_qk / torch.sqrt(torch.tensor(dk, dtype=torch.float32))
        if mask is not None:
            scaled_attention_logits += (mask * -1e9)
        attention_weights = nn.functional.softmax(scaled_attention_logits, dim=-1)
        output = torch.matmul(attention_weights, v) 
        return output, attention_weights
```

The outputs of the `scaled_dot_product_attention` are `output` and `attention_weights`, with outputs having dimension of `(batch_size, num_heads, seq_len, depth)`.

## Feed-Forward Network

After attention, the model applies a `feed-forward network` to each word's representation independently. This network consists of two linear transformations with a `ReLU` activation in between. The `FFN` operates on each word independently but helps capture more complex interactions within each word's updated representation from the attention mechanism. The output from the `FFN` is passed back to the `multi-head attention` layer for additional iterations. Multiple layers of attention and `FFN` are stacked on top of each other to deepen the model's understanding.

## Residual Connections and Layer Normalization

To help the model learn more effectively, the Transformer uses `residual connections` and `layer normalization`. These mechanisms ensure that the model retains the original information at each step and helps stabilize learning. To help the model learn effectively, the Transformer adds `residual connections` around the attention and `feed-forward` layers. This means the input to each layer is added to the layer's output. Each time the data passes through `self-attention` or the `feed-forward network`, the input is added back to the output (`residual connection`), and the result is normalized. This keeps the information flowing through the network and helps with training stability. `Residual connections` are added around both the `self-attention` and `feed-forward` layers to maintain data integrity. These normalized outputs move between each of the stacked layers. The combined multi-head attention with `feed-forward network` and `layer normalization` can be implemented similar to below snippet, where `self.fnn` is the `feed-forward network` and `self.layernorm1` and `self.layernorm2` are layer normalizations over attention output and feedforward network outputs.

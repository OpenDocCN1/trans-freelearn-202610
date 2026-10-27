es into token sequences: Patch → token, Image → sequence, Transformer → global reasoning engine. Unlike CNNs, which operate locally (relying on convolutional inductive bias: locality and translation equivariance), transformers allow all patches to interact directly (via global context modeling).

+   - **Positional encoding:** Transformers lack inherent spatial awareness, to make them permutation invariant, positional information needs to be injected:
+   
+     $$Z_0 \leftarrow Z_0 + E_{pos},\quad E_{pos} \in \mathbb{R}^{N\times D}$$

Design choices vary: e.g., *ViT* / *DPT* has *learnable embeddings*, *Swin* has *relative positional bias*, whereas *SegFormer* removes explicit *positional encoding*. The position tells the model: “Where is this patch located in the image?” Without it, spatial structure is lost. Without positional encoding a pixel at the **top-left** is indistinguishable from one at the **bottom-right**. Positional encoding restores **geometry**, enabling object boundaries, spatial relationships and scene layout understanding.

+   - **Self-Attention (Core Mechanism):** The fundamental operation is:
+   
+     $$\operatorname{Attention}(Q,K,V)=\operatorname{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right)V$$

This equation tells us: Each token decides how much it should “*pay attention*” to every other token. Think of each token as a patch of the image.

From: $Z \in \mathbb{R}^{N\times D}$, we compute:

$$Q=ZW_Q,\quad K=ZW_K,\quad V=ZW_V.$$

The next table explains the meaning Q, K and V:

| Symbol | Meaning | Intuition |
|---|---|---|
| Q (Query) | “What am I looking for?” | Each token asks a question |
| K (Key) | “What do I contain?” | Each token describes itself |
| V (Value) | “What information do I provide?” | Actual content |

*Table 10.2: Query–Key–Value in a transformer*

This enables global interaction, where each patch queries all other patches, measures similarity and aggregates relevant information. Thus, every pixel can “see” the entire image. It can be thought of as conversation between pixels, each patch asks a question (**Query**), compares with all other patches (**Key**) and gathers relevant information (**Value**). This enables **context-aware segmentation**, unlike CNNs that rely on local neighborhoods.

Next figure illustrates the **Scaled Dot-Product Attention mechanism**, where input tokens are transformed into **Query (Q), Key (K), and Value (V)** vectors. It shows how similarity scores $(QK^T)$ are computed, scaled, normalized via softmax, and used to produce a **weighted sum of values**, enabling each token to attend to most relevant parts of input.
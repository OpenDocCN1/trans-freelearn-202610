![](bbox=[0.273, 0.074, 0.716, 0.29])

As you can see, **GrabCut** solves segmentation as:

$$\text{Segmentation} = \arg\min E(f)$$

using **probabilistic modeling + graph optimization**.

# Conditional random fields

Conditional random fields (CRFs) model segmentation as a **probabilistic labeling problem with contextual dependencies**, where each pixel’s label depends not only on its own features but also on its neighbors.

# Probabilistic model

Intuitively, a CRF improves segmentation by refining each pixel’s label based on the agreement with its neighboring pixels, ensuring spatially coherent regions rather than independent per-pixel predictions.

Given image $I$, we model label assignment $\mathbf{X}=\{x_i\}$, where $x_i$ is label of pixel $i$ (e.g., foreground/background or semantic class) and $P(\mathbf{X}\mid I)=\frac{1}{Z}\exp(-E(\mathbf{X}))$, where $Z$: **partition function** (normalizes probabilities), and $E(\mathbf{X})$: **Gibbs energy**, measuring how good a labeling is, where low energy → high probability. CRF chooses the labeling that minimizes energy (maximum a posteriori, MAP).

**Energy function:**

$$E(\mathbf{X}) = \sum_i \psi_u(x_i) + \sum_{i<j} \psi_p(x_i,x_j),$$

where $\psi_u(x_i)$ – unary potential (data term): Measures how well the pixel $i$ fits label $x_i$. It is often derived from a classifier (e.g., CNN output or annotation), in the following implementation, it is derived directly from the annotation labels.

$\psi_p(x_i,x_j)$ – pairwise potential: Encourages spatial consistency between pixels.

Unary → “what this pixel wants to be” and Pairwise → “what its neighbors want it to be”.
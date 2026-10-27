2. **Convert annotation to labels:** Converts RGB → discrete labels $x_i \in \{1,2, ..., K\}$ (integer classes), where $K$ is the number of unique labels in the annotation. This defines the **label space** of the CRF.

3. **Handle unknown labels:** Some pixels may have **unknown labels** (HAS_UNK = 0 in colors). These contribute weakly to unary term. CRF relies more on **pairwise terms** for them.

4. **Initialize dense CRF model:** Fully connected CRF:

$E(\mathbf{X}) = \sum_i \psi_u(x_i) + \sum_{i<j} \psi_p(x_i, x_j)$

where every pixel interacts with **every other pixel**, the DenseCRF() class constructor instantiates a **CRF** object, as shown in the next code snippet.

5. **Unary potential (Data Term):**

$\psi_u(x_i) = -\log P(x_i)$

derived from annotation using the function unary_from_labels(), with the confidence controlled by gt_prob (0.7 in the code). Use the method setUnaryEnergy() to set **unary potentials** $\psi_u(x_i)$, to encode initial belief (data term) for each pixel label, where strong annotation → strong unary and weak/unknown → CRF can correct.

# **Pairwise potentials (smoothness and context)**

In practice, pairwise potentials are commonly implemented using *Gaussian* and bilateral *kernels*, which encourage *spatial consistency* while preserving important image boundaries and contextual relationships between pixels.

+   - **Gaussian Kernel (Spatial Smoothing):** Use the function create_pairwise_gaussian() to compute $\exp\left(-\frac{|p_i - p_j|^2}{2\sigma^2}\right)$, depends only on spatial distance. Use the method addPairwiseEnergy() to add it to the CRF.
+   - **Bilateral Kernel (Edge-Aware Smoothing):** Use the function create_pairwise_bilateral() to compute $\exp\left(-\frac{|p_i - p_j|^2}{2\sigma_a^2} - \frac{|I_i - I_j|^2}{2\sigma_b^2}\right)$. Use the method addPairwiseEnergy() again to add it to the CRF. If pixels are **close + similar color → strong interaction** and **different color → weak interaction**. It preserves edges while smoothing.

Use the method addPairwiseEnergy() to add a **pairwise potential term** $\psi_p(x_i, x_j)$ to the **CRF**. Here is how it works and the parameters it accepts are feats (feature vectors, e.g., spatial or bilateral features), compat (weight w, strength of smoothing: Computes similarity between pixels using Gaussian kernels and defines *how strongly pixels influence each other*). **Gaussian** → smooth nearby pixels (spatial smoothness), whereas **bilateral** → smooth only similar-color pixels (edge-preserving refinement), encourages **nearby pixels → same label** and removes noise.

**Mean-Field Inference:** It's an efficient approximation that converts hard optimization into iterative filtering and probabilistic updates. Use the method inference(n_iter) to run **mean-field inference** for n_iter (e.g., 5 iterations in the code) iterations and update:

$Q_i(x_i) \propto \exp\left(-\psi_u(x_i) - \sum_j \psi_p(x_i, x_j)\right)$
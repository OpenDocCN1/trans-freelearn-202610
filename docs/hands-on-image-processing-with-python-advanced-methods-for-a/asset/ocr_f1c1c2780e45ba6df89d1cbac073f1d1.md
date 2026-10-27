# Energy Formulation:

E(f, θ) = Σₚ Dₚ (fₚ | θ) + Σ₍p,q₎∈N Vₚq (fₚ, f_q),

where θ: model parameters (e.g., foreground/background GMM parameters like means, variances, and mixture weights).

Dₚ(fₚ): Data Term (Unary Term), which measures how well pixel p fits the assigned label fₚ. Typically, Dₚ(fₚ) = −logP(Iₚ | fₚ), negative log-likelihood from GMM. If pixel intensity Iₚ looks like foreground → low cost for fₚ = foreground and if not → high cost. In simple words, it refers to how likely is this pixel to belong to this class.

Vₚq(fₚ, f_q): Smoothness Term (Pairwise Term), which penalizes neighboring pixels (p,q) having different labels:

Vₚq(fₚ, f_q) = γ · [fₚ ≠ f_q] · exp(−β|Iₚ − I_q|²),

where fₚ, f_q : Labels assigned to pixels (e.g., foreground/background), Iₚ, I_q: Pixel intensities or color values. If two pixels look similar → strong penalty for assigning different labels and if very different → weak penalty.

In simple words, nearby similar pixels should have the same label. Here γ: controls strength of smoothness and β: controls sensitivity to intensity differences. Again, Data term → individual pixel decision, Smoothness term → neighborhood consistency, Segmentation balances: “Fit the data” + “Keep regions smooth”.

# Optimization via Graph Cuts

First construct a graph: Nodes → pixels and Edges → neighborhood connections. Add Source (foreground) and Sink (background). Solve using min-cut / max-flow:

min_f E(f) ⇔ minimum cut.

# Iterative refinement

GrabCut alternates between estimating GMM parameters θ and minimizing energy via graph cut. This is similar to an EM (Expectation Maximization)-like optimization.

# GrabCut with OpenCV-Python

GrabCut is an interactive image segmentation method based on graph cuts (max-flow/min-cut) that separates foreground from background using minimal user input. The user typically provides a loose bounding box around the object, after which the algorithm iteratively refines the segmentation. If the result is imperfect, additional user scribbles marking foreground or background pixels can be provided to correct errors and further improve the segmentation quality. The next code snippet demonstrates how the flowers (foreground objects) from an image can be segmented from the background using this algorithm. Here are the steps:

+   1. Load Image: First read the input dahlia flowers image with cv2.imread() function (remember it reads an RGB image in BGR format).
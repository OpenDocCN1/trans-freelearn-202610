# ## 4

**The Architecture of the Object Detection Model in Detectron2** — 65

+   - Technical requirements — 65
+   - Introduction to the application architecture — 66
+   - The backbone network — 68
+   - Region Proposal Network — 72
+   - The anchor generator — 74
+   - The RPN head — 77

+   - The RPN loss calculation — 78
+   - Proposal predictions — 81
+   - Region of Interest Heads — 83
+   - The pooler — 85
+   - The box predictor — 86
+   - Summary — 89

# ## 5

**Training Custom Object Detection Models** — 91

+   - Technical requirements — 91
+   - Processing data — 92
+     - The dataset — 92
+     - Downloading and performing initial explorations — 92
+     - Data format conversion — 97
+     - Displaying samples — 98
+   - Using the default trainer — 100

+   - Selecting the best model — 103
+   - Evaluation metrics for object detection models — 103
+   - Selecting the best model — 104
+   - Inferencing thresholds — 108
+   - Sample predictions — 109
+   - Developing a custom trainer — 111
+   - Utilizing the hook system — 112
+   - Summary — 115

# ## 6

**Inspecting Training Results and Fine-Tuning Detectron2’s Solvers** — 117

+   - Technical requirements — 117
+   - Inspecting training histories with TensorBoard — 118
+   - Understanding Detectron2’s solvers — 122
+     - Gradient descent — 122
+     - Stochastic gradient descent — 125

+   - Momentum — 132
+   - Variable learning rates — 134
+   - Fine-tuning the learning rate and batch size — 137
+   - Summary — 138
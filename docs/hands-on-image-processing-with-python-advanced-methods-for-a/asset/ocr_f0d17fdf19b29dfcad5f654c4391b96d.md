# Note

Dice ranges from 0 to 1. Higher Dice indicates stronger overlap. Dice is more sensitive to small structures than IoU. This property makes Dice particularly effective for tumor segmentation, lesion detection, vessel segmentation, and brain MRI analysis.

# Dice loss

Deep learning models often optimize the Dice loss:

```
L_Dice = 1 − Dice
```

or its differentiable soft version:

```
L_Dice = 1 − (2 Σ_i p_i g_i + ε) / (Σ_i p_i + Σ_i g_i + ε)
```

where p_i is the predicted probability, g_i is the ground-truth label and ε prevents division by zero. Dice loss is highly effective for imbalanced datasets where the foreground occupies only a small image region.

# Pixel accuracy

Pixel accuracy measures the proportion of correctly classified pixels:

```
Accuracy = correct pixels / total pixels
```

For binary segmentation,

```
Accuracy = (TP + TN) / (TP + TN + FP + FN)
```

Pixel accuracy evaluates global classification correctness but may be misleading for highly imbalanced datasets. Sometimes it may happen that the accuracy is high, but the segmentation is completely useless because the object is never detected. Therefore, pixel accuracy should not be used alone for segmentation evaluation.

# Mean pixel accuracy

For multiclass segmentation, we have:

```
Mean Accuracy = (1/C) Σ_{c=1}^C TP_c / (TP_c + FN_c)
```

where C is the number of semantic classes.
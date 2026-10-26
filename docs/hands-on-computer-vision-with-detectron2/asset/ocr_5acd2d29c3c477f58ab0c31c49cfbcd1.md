```
print('negative', sum([x == 0 for x in na]))
print('ignore', sum([x == -1 for x in na]))
```

Specifically, this code snippet gets the labels and counts the positive, negative, and ignored anchors based on labels. This code snippet should display the following output:

```
positive 42
negative 214
ignore 37244
```

Thus, out of 37,500 anchors, 42 are positive, and 214 are negative. These 256 anchors contribute to the loss calculation (training), and the rest are ignored. Now, the RPN loss calculation is the weighted contributions between the classification loss (based on negative and positive labels of the sampled anchors and their respective predictions) and the regression loss (based on the ground-truth bounding box locations and their respective predicted anchor deltas). After all the previous steps, the next task of the RPN is to make the proposals. The following sections inspect the way RPN proposes regions.

# Proposal predictions

The proposal prediction step has a new concept called Non-Maximum Suppression (NMS). Specifically, there can be multiple proposals made for one ground-truth box. These proposals may overlap in a large area and have different objectness scores. Figure 4.10 shows an example of why NMS is necessary. Specifically, two proposals (dashed boxes) are made for one ground-truth box (solid one). Observably, these two proposals have a large overlapping area, and it is a good idea to remove one and keep another one with a higher objectness score (higher confidence).

![](bbox=[0.21, 0.58, 0.75, 0.86])

Figure 4.10: A ground-truth label (solid box) and multiple proposals (dashed boxes)
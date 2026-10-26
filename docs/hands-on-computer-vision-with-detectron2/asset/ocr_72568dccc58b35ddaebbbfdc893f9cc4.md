The code for each step is explained in the following sections. Specifically, the code for step 1 generates the anchors using the current sizes and ratios and computes the current fitness score:

```
anchors = generate_anchors(tuple(sizes), tuple(ratios))
ac_wh = boxes2wh(anchors)
anchor_shape = len(sizes) + len(ratios)
best_fit = fit_fn(ac_wh, gt_wh)
```

The following code snippet provides the codes for steps 2 and 3. These steps randomly sample items to mutate and the amounts for mutation:

```
mutation = np.ones(anchor_shape)
mutate = np.random.random(anchor_shape) < probability
mutation = np.random.normal(muy,sigma,anchor_shape)*mutate
mutation = mutation.clip(0.3, 3.0)
```

Steps 4 and 5 make changes to the sizes and ratios after getting information about what to change and how much to change. Additionally, it also computes the new fitness score after performing mutation. The source code for these steps is as follows:

```
mutated_sizes = sizes.copy()*mutation[:len(sizes)]
mutated_ratios = ratios.copy()*mutation[-len(ratios):]
mutated_anchors = generate_anchors(
    tuple(mutated_sizes),
    tuple(mutated_ratios))
mutated_ac_wh = boxes2wh(mutated_anchors)
mutated_fit = fit_fn(mutated_ac_wh, gt_wh)
```

Lastly, if the new fitness score is better, the code for step 6 updates the best score, sizes, and ratios with their corresponding current values:

```
sizes = mutated_sizes.copy()
ratios = mutated_ratios.copy()
best_fit = mutated_fit
msg=f"Evolving ratios and sizes, Fitness = {best_fit:.4f}"
pbar.desc = (msg)
```

We can then run the following line of code to start the optimization process:

```
e_sizes,e_ratios = evolve(sizes,ratios,gt_wh,verbose=False)
```
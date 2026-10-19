+   6. Press the green **OK** check mark to apply the unfolding.

The **Unfold** feature now appears in the FeatureManager Design Tree. In this unfolded state, we could now make changes to the part, such as cutting across bends.

Note that the unfolded model looks exactly the same as a *flattened* model would; however, there are some important differences that are explained in more detail in the next two sections.

The **Fold** feature works in the opposite way to the **Unfold** feature—so, all of the steps are reversed:

+   1. Select the **Fold** feature.
+   2. Select the **Fixed Face**. In this case, this is the same as the **Unfold** example earlier—that is, the square **Base Flange**.
+   3. Select the bends to be folded. These can be selected manually, or the **Collect All Bends** button can be used to select all of the appropriate bends in the model.
+   4. Press the green **OK** check mark. The model is re-folded and the **Fold** feature appears in the FeatureManager Design Tree.

In the next section, we'll explain why the **Unfold** and **Fold** features might actually be needed in Sheet Metal modeling.

# Why use Fold/Unfold?

The **Unfold** and **Fold** features can be very useful when making cuts across bends that could not be made in the folded state. Here's an example of how this can be done:

+   1. Continuing on from the previous model, delete the **Fold** feature, but leave the earlier **Unfold** feature. This should leave an unfolded model with a rough cross shape.
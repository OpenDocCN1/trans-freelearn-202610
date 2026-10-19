> **Reality check – don’t give up on simple classifiers too easily**
> 
> To clarify that last statement, since we need to keep our examples simple, for this dataset simple classifiers such as **support vector machines** or **k-nearest neighbors** would probably still give you good enough results. I encourage you to try these classifiers first on your own data.
> 
> However, at some stage, you’re going to encounter a dataset that is too complicated for these algorithms, which is where the power of neural networks is useful.

# A neural network implemented in Excel

Neural networks are a big deal, and you’ve almost certainly heard of them already. You may be surprised that we can implement one in Excel, as shown in *Figure 12.4*. The actual Excel file is available on the book’s GitHub site as `12_Neural_Network.xlsx`:

![](bbox=[0.131, 0.359, 0.854, 0.64])

You should be able to easily understand the concept as you can work through it in Excel:

+   - Normally, all we care about are the **inputs** at the top of sheet in green (**Customer Age** and **Country Code**) and the recommendation **output** at the end of the sheet (also in green). The output is the percentage chance that our customer will buy a Peanut Crunch bar.
+   - To calculate our output, we pass the inputs through three neurons each with the same pattern. We have different weightings that we can set within each neuron (the light gray box).
+   - Those weighting values allow us to modify how much importance we put on each input, which we use to calculate our **Inputs after weighting** field.
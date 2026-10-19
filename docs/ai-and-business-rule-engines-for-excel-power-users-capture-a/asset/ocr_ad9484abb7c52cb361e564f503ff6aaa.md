# Step-by-step training of the Machine Learning model

Whether we have our sample notebook (11-naive_bayes.ipynb) open in Azure ML or in another viewer, you should see a screen similar to the one shown in Figure 11.14:

![](bbox=[0.14, 0.2, 0.87, 0.48])

Figure 11.14 – Our ML notebook hosted in Azure

The notebook format for ML (with text, executable cells, output, and a button that executes each cell) is exactly the same as the 3D graph notebook we saw earlier. So, we'll focus on walking through the notebook step by step, and assume that you're pressing the Execute ▶ button each time to run each step in the notebook. If you're impatient, there is also an option at the top of the notebook to run all the steps at once—you'll still see the output of each step.

The notebook format means that each step is very well documented, so we'll walk through at a high level what is going on:

+   1. The first cell makes sure we have access to the ML toolkits we need to run our notebook. When you run this step for the first time, you'll have to wait for a couple of minutes as Python downloads the required libraries. It will be faster from the second time onward:

```
pip install numpy matplotlib seaborn scikit-learn nyoka sklearn2pmml
```
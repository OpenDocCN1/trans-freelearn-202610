```
## Test model README
Some test README description
```

The top part of the preceding snippet, wrapped in the --- symbols, contains metadata that tells Hugging Face that this is a sequence tagging model trained by Flair. It also defines some default placeholder text for the browser widget. The bottom part of the snippet simply defines the part of the snippet visible to humans and acts as a description.

Make sure to click **Commit new file**, which will save the model card as README.md and make the description visible on the **Model card** tab.

We're now ready to test our model.

# Using the model as part of the Accelerated Inference API

At this point, your Hugging Face model repository should contain these two files:

+   - `pytorch_model.bin`
+   - `README.md`

The latter will tell Hugging Face that we're dealing with a sequence tagging model trained by Flair, and the former is the actual model binary that the hub will use.

If we did everything right, the model will, by default, be available on the Accelerated Inference API. Best of all, it can be tested using the browser widget with no code required.

If you navigate to the **Model card** tab, you will see the widget on the right-hand side, where you can input some random text. Click **Compute** to perform inference and produce some results, as shown in the following screenshot:

![](bbox=[0.115, 0.575, 0.845, 0.785])

Figure 9.3 – The Hugging Face Models Hub widget producing test results
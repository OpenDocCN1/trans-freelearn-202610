The first and probably most important thing to monitor during training is the trend of how a loss value is changing. A decrease in loss usually means the model is learning. From the previous output, we can see that the first epoch finished with a loss of 69.5423, and the second epoch finished with a loss of 59.1209. This indicates that the model is learning! The output also indicates that our accuracy score increased from 0.0654 to 0.1115.

If you remember, earlier in this section, we mentioned that the sequence tagger objects are initialized with random weights. This means that when you run the same exact code twice, the values will be slightly different.

Training, unless interrupted, will now continue to pass through a number of epochs until it does max_epochs, or until the trainer fails to improve the model for patience consecutive epochs.

On an average GPU, training a downsampled English dataset should finish within a few minutes. When it does, Flair will report the final results of the best model tested on the test dataset. The results should look something like this:

```
Results:
- F-score (micro): 0.8878
- F-score (macro): 0.702
- Accuracy (incl. no class): 0.8878
```

After these results, we should also see results reported for each tag individually.

Training is now complete. We can now try to tag the same pair of words (hello world) sentence on our newly trained model to see whether the results are now any better:

```
tagger.predict(sentence)
print(sentence)
```

This code will, if you followed the steps correctly, print out the following:

```
Sentence: "Hello world" -> ["Hello"/UH, "world"/NN]
```

The UH tag in the output stands for interjection and NN for noun. This means that our trained tagger labeled our example sentence correctly!
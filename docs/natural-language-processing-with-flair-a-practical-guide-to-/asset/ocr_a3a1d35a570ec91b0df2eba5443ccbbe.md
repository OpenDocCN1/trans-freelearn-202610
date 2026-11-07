If we compute the F1 score for all the tags counting the total true positives, false negatives, and false positives together, we obtain the *micro F1 score*. This metric will have the same value for accuracy as single-label problems (for taggers where one token can have at most one tag assigned).

Next time you assess reported sequence tagger results, be careful about whether the F1 score refers to the micro or the macro variant. The variant most often used in Flair's results (when not explicitly specified) is the micro F1 score.

# Summary

With the knowledge gained in this chapter, we are not only able to use many of the pretrained sequence taggers available in Flair, but we are also capable of interpreting and understanding their output.

In this chapter, we covered sequence tagging in Flair from different angles. We touched on the design and architecture of Flair's proposed sequence taggers. We then covered NER motivation and theory, and what pretrained NER taggers can be found in Flair. We did the same with PoS tagging, where we covered the most notable tag sets, such as the Penn tag set and the universal PoS tag set, before covering the important pretrained PoS taggers found in Flair. We then finally studied the two metrics most often used to evaluate sequence taggers, and we emphasized the importance of distinguishing between micro and macro F1 scores.

In the next chapter, we will be looking into using Flair to train, store, and reuse your own sequence labeling models.
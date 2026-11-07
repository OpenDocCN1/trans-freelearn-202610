# 4

# Sequence Tagging

**Sequence tagging** (or **sequence labeling**) refers to a set of **Natural Language Processing** (**NLP**) tasks that assign labels or tags to tokens or other units of text. When the tags are named entities, we are then dealing with **named entity recognition** (**NER**). When the tags are parts of speech, this task is called **part-of-speech** (**PoS**) tagging. Unlike embeddings that are trained in an unsupervised manner, sequence taggers are trained using supervised training techniques, making them easier to evaluate and compare.

**Sequence tagging** is a field where **Flair** truly shines. **Flair** uses the ingenuity of **Flair** embeddings (explained in the previous chapter) to achieve state-of-the-art results across many different sequence tagging tasks and languages.

In this chapter, we are going to briefly explain how sequence taggers work in **Flair**. This will allow us to fully understand the inner workings as we cover **NER**, **PoS** tagging, chunking, and other sequence tagging techniques found in **Flair**. The knowledge gained in these sections will help you understand the range of sequence tagging tasks solved by **Flair** and what pretrained models are available for you to make use of. We will then show what metrics are generally used in **NLP** to measure and compare the performance of sequence taggers.

We will cover sequence tagging in Flair in the following sections:

+   - Understanding how sequence tagging works in Flair
+   - Named entity recognition in Flair
+   - Part-of-speech tagging in Flair
+   - Other sequence taggers in Flair
+   - Sequence labeling metrics
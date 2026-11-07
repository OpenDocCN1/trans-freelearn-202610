+   - Training a text classifier in Flair
+   - Working with text classifiers that require little to no training examples

Let's dive in!

# Technical requirements

All of the Python libraries used in this chapter are direct dependencies of Flair version 0.11 and require no special setup, assuming Flair is already installed on your machine. Code examples covered in this chapter are found in this book's official GitHub repository in the following Jupyter notebook: https://github.com/PacktPublishing/Natural-Language-Processing-with-Flair/tree/main/Chapter08.

You will be able to run most of the code and train most models in this chapter using a CPU. However, the final exercise does require a GPU-equipped machine.

# Understanding text classification

Text or document classification is simply a process of assigning one or more labels (often called classes) to a piece of text (often called a document). A text classifier is a machine learning model that receives some text as input and computes a probability distribution over a set of classes.

Text classification has many real-world uses and is actively used to solve the following problems:

+   - **Topic classification** – the process of assigning a topic to a document
+   - **Spam detection** – the process of detecting unwanted emails
+   - **Sentiment analysis** – classifying text sentiment into positive and negative
+   - **Hate speech detection** – identifying hate speech in text
+   - **Language identification** – figuring out what language a document is written in

Here's an example of the real world use of text classification:

![](bbox=[0.35, 0.75, 0.68, 0.9])

Figure 8.1 – Spam detection
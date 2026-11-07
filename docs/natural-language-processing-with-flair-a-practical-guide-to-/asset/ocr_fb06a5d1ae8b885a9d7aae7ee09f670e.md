# 3

# Embeddings in Flair

Embeddings are a core concept of **Natural Language Processing (NLP)** tasks in **Flair**. Understanding how different embeddings work, what types are available and knowing what type to use in what scenario is important in becoming a seasoned Flair user.

While the framework offers a large selection of different embedding types, there is one that is absolutely essential and plays a pivotal role in achieving Flair's excellent performance on NLP tasks. This type of embeddings, called Flair embeddings, is referred to by the main author as *the secret sauce of Flair*. Understanding the ins and outs of Flair embeddings will help you better understand the success behind Flair across so many NLP tasks and allow you to choose the right embeddings when training custom models.

In this chapter, we will learn about the different embedding types, how they work, and how to use the individual embedding techniques in Flair for producing word, sentence, and document embeddings. After reading this chapter, you will get a deeper understanding of what embeddings to use in what situation. We will demonstrate how you can use arithmetic operations on word embeddings to understand the meaning and relationships between words using the "king is to man as queen is to woman" analogy. We will learn how to form new embedding types by combining multiple existing embeddings using stacked embeddings. We will also cover document embeddings and how to use them to produce embeddings for entire documents.
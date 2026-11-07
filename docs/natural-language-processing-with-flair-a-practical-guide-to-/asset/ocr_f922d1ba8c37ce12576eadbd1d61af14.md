# Obtaining the embeddings for words A, B, and C and computing the embedding D

As we learned in previous chapters, we can embed words in Flair by first wrapping them in the `Sentence` object. For example, given a `fasttext` object such as the one defined previously, our `Sentence` object can be defined with `sentence = Sentence('word')`. Then, we can embed the words in our `Sentence` object by running `fasttext.embed(sentence)`. We can also obtain the embedding for the first word in the sentence by calling `sentence[0].embedding`. This will return a **PyTorch** `torch.Tensor` object. It can be turned into a list of vector components by calling `sentence[0].embedding.tolist()`.

With this knowledge, we can implement a `compute_embedding_for_D(A, B, C, embedding)` function that receives our three words (A, B, and C) and an embedding object and returns the embedding vector of the computed word D.

The implementation of this method is as follows:

```
```python
from flair.data import Sentence

def compute_embedding_for_D(A, B, C, embedding):
    wordsABC_sentence = Sentence(' '.join([A, B, C]))
    embedding.embed(wordsABC_sentence)

    A_embedded = wordsABC_sentence[0].embedding
    B_embedded = wordsABC_sentence[1].embedding
    C_embedded = wordsABC_sentence[2].embedding

    D_embedding = B_embedded + C_embedded - A_embedded

    return D_embedding.tolist()
```
```

In the first part of this helper function, we define the `wordsABC_sentence` object, which contains a space-separated string containing our three input words, A, B, and C. We then proceed to embed all the words in that sentence and extract embeddings for each of our three input words and thus obtain our word embeddings called `A_embedded`, `B_embedded`, and `C_embedded`. Using our $D = B + C - A$ formula and Python's arithmetic operators, we then simply compute `D_embedding` and return it as a list.
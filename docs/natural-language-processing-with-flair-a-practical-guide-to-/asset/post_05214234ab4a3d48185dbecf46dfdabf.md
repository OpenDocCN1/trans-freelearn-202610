
We can now compute the embedding for the word D given the embeddings for words A, B, and C. Once we get the embedding for the word D, finding its corresponding word is easy. We simply search embeddings of the entire vocabulary and find the word with the closest matching embedding.

It's now time we put our analogy solver to the test with Flair.

## Implementation

We are going to implement an algorithm that, given the words A, B, and C, computes the word D, in such a way that the A is to B as C is to D analogy will hold. For example, when given the input King is to man as queen is to..., the algorithm will respond with woman. The algorithm will consist of the following steps:

- 1. Choosing and instantiating the embedding class we will be using in Flair
- 2. Obtaining the embeddings for words A, B, and C
- 3. Computing the approximate embedding for the word D using the D = B + C − A formula
- 4. Obtaining embeddings for all English words in Flair
- 5. Finding the closest matching word

### Choosing and initializing the embedding class

When choosing embedding types, it's important that we choose a technique that isn't contextual because we will be computing new words independently of the context we might find them in.

We will therefore choose a set of random classic word embeddings using the crawl ID. The crawl embeddings are English word embeddings trained over web crawl data. The embedding object can simply be initialized by running the following:

```
from flair.embeddings import WordEmbeddings
fasttext = WordEmbeddings('crawl')
```

The first time this code is executed, Flair will download the embedding model, which takes up some space and may take a few minutes to download fully.

To save us from having to download the model again, Flair will make sure to use the locally stored version next time we use it.

Once the embedding object is loaded, we can keep it in memory and reuse and pass it to our helper functions down the line to save execution time.

### Obtaining the embeddings for words A, B, and C and computing the embedding D

As we learned in previous chapters, we can embed words in Flair by first wrapping them in the `Sentence` object. For example, given a `fasttext` object such as the one defined previously, our `Sentence` object can be defined with `sentence = Sentence('word')`. Then, we can embed the words in our `Sentence` object by running `fasttext.embed(sentence)`. We can also obtain the embedding for the first word in the sentence by calling `sentence[0].embedding`. This will return a **PyTorch** `torch.Tensor` object. It can be turned into a list of vector components by calling `sentence[0].embedding.tolist()`.

With this knowledge, we can implement a `compute_embedding_for_D(A, B, C, embedding)` function that receives our three words (A, B, and C) and an embedding object and returns the embedding vector of the computed word D.

The implementation of this method is as follows:

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

In the first part of this helper function, we define the `wordsABC_sentence` object, which contains a space-separated string containing our three input words, A, B, and C. We then proceed to embed all the words in that sentence and extract embeddings for each of our three input words and thus obtain our word embeddings called `A_embedded`, `B_embedded`, and `C_embedded`. Using our $D = B + C - A$ formula and Python's arithmetic operators, we then simply compute `D_embedding` and return it as a list.

To test our method, we can run the following:

```python
from flair.embeddings import WordEmbeddings
fasttext = WordEmbeddings('crawl')
D = compute_embedding_for_D('king', 'man', 'queen', fasttext)
print(D)
```

This should print out our D embedding – a long list of numbers looking something like this:

```
[0.13569998741149902, -0.3856000006198883, -0.102299957203865,
-0.13539999723434448, -0.01410001516342163,
-0.04699999839067459, ...]
```

Given our King is to man as queen is to woman analogy, the resulting embedding should be similar to the embedding of the word woman. To verify that, we need to first compute embeddings of our English vocabulary and then find the closest matching word.

### Obtaining embeddings for all English words in Flair

To obtain a list of the words we can, to a reasonable extent, expect in the English language, we are going to extract a vocabulary from a large English corpus. This way, we are going to get a near-complete set of commonly used English words while getting rid of the rarely used words in English which we would be extremely unlikely to ever stumble upon, thus reducing our search space and speeding up our code.

We will then embed those words as part of a single sentence containing a space-delimited list of our vocabulary. This can only be done when using non-contextual word embeddings. If we were to use contextual string embeddings, we would have to define one Sentence object for each word separately.

We will implement the solution as the get_embedded_english_vocab(embedding) function, which receives the embedding object and returns a Sentence object containing our entire vocabulary as tokens along with their corresponding embeddings:

```python
from flair import datasets
from flair.data import Sentence

def get_embedded_english_vocab(embedding):
    dataset = datasets.UD_ENGLISH()
    vocab_list = dataset.make_vocab_dictionary().get_items()
```

```
vocab = Sentence(' '.join(vocab_list))
embedding.embed(vocab)
return vocab
```

The get_embedded_english_vocab(embedding) function uses Flair's UD_ENGLISH Universal Dependency Treebank for English dataset. It concatenates the dataset's entire vocabulary into a space-separated string that is then tokenized and embedded as part of the Sentence object.

The preceding function can be tested by running the following:

```
from flair.embeddings import WordEmbeddings
fasttext = WordEmbeddings('crawl')
print(get_embedded_english_vocab(fasttext)[6].embedding)
```

Here, we initialize a word embedding class and pass it to the get_embedded_english_vocab function. The snippet will then finally print out the embedding of the seventh word in our vocabulary.

Given that we now have access to embeddings of all the words we can reasonably expect in the English language, we can now compute the word closest to our computed D word embedding.

### Finding the closest matching word

Once we've computed the D word embedding, we finally need to find the word that our embedding represents. Because the embedding is highly unlikely to be an exact match with a word from our vocabulary, we need to find the closest matching word using some sort of a similarity measure. We will use the cosine similarity. To find the closest matching word, we will simply compute the cosine similarity between D and every word in our vocabulary and choose the word with the maximum similarity score. This will be implemented as find_closest_matching_word(D, vocab, ABC), where D is our computed embedding, vocab is our embedded vocabulary, and ABC is a list of the words A, B, and C:

```python
from sklearn.metrics.pairwise import cosine_similarity as sim
def find_closest_matching_word(D, vocab, ABC):
    max_match = -1
```

```
for word in vocab:
    match = sim([D], [word.embedding.tolist()])[0][0]
    if match > max_match and word.text not in ABC:
        max_match = match
        closest_matching_word = word.text
return closest_matching_word
```

The preceding code simply iterates through our vocab object and computes the similarity between the D embedding and each word in the vocab object. We then pick and return closest_matching_word, which is the solution to our analogy solver.

> Tip
> While our idea of using arithmetic operations to solve analogies makes sense in theory, it in practice often turns out that the results aren't as sensible as you would expect. What often happens is that the newly computed embedding is actually closest to one of our original words. Of course, we want D to be a new word and not one of the original words, A, B, or C. When this happens, we can simply ignore the closest matching word and choose the second, third, or fourth closest matching word. We do this by checking for and word.text not in ABC in the preceding if statement and thus avoid the result being one of the original words and increase our chances of producing a sensible result.

Having implemented all the required logic for our analogy solver, we can now write a simple helper function that integrates all the functions together and print out the solution to our analogy puzzle:

```python
def A_is_to_B_as_C_is_to(A, B, C):
    fasttext = WordEmbeddings('crawl')
    result = compute_embedding_for_D(A, B, C, fasttext)
    vocab = get_embedded_english_vocab(fasttext)
    D = find_closest_matching_word(result, vocab, {A, B, C})
    
    print(f'{A} is to {B} as {C} is to {D}')
```

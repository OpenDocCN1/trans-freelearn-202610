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

```
def A_is_to_B_as_C_is_to(A, B, C):
    fasttext = WordEmbeddings('crawl')
    result = compute_embedding_for_D(A, B, C, fasttext)
    vocab = get_embedded_english_vocab(fasttext)
    D = find_closest_matching_word(result, vocab, {A, B, C})
    
    print(f'{A} is to {B} as {C} is to {D}')
```
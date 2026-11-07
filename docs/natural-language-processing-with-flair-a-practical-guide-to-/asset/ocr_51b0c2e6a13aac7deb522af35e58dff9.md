A similar string representation of the Sentence object can also be directly obtained by calling the to_tagged_string() method.

> **Important Note**
> The get_token(n) method uses 1-based indexing, meaning that n=1 will retrieve the first token, whereas n=0 will return None. A 0-based indexing alternative to using get_token(n) would be to use the indexer operator. For example, to get the first token for a Sentence object called sentence, we can simply call sentence[0].

Tokens in a tokenized Sentence object can also be obtained by using its __iter__() magic function, which allows us to get all the tokens by simply iterating the object. We can, for example, do that by using a for loop.

Let's run the following script using the same sentence variable from the preceding script:

```
for token in sentence:
    print(token)
```

The script will print out the following:

```
Token[0]: "A" → DT (1.0)
Token[1]: "short"
Token[2]: "sentence"
```

This allows us to get a clear visual representation of how our original string was tokenized.

When working with the Sentence object, it is important to understand the behavior of using the len() method with the object. It will return the number of tokens, not the length of the original string. This means that calling len(sentence) using the variable in the preceding script would return 3. If you wish to get the length of the actual sentence (that is, the original text) you will need to run len(sentence.to_original_text()).

If none of the tokenizers described here work for you, Flair has you covered. All you need to do is implement your own custom tokenizer and pass it into Flair. Let's learn how.

# Using custom tokenizers

While Flair ships with several tokenizers that support the most commonly spoken languages, it is entirely possible you will be working with a language that uses tokenization rules currently not covered by Flair. Luckily, Flair offers a simple interface that allows us to implement our tokenizers or use third-party libraries.
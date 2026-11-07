# Using the TokenizerWrapper class

The TokenizerWrapper class provides an easy interface for building custom tokenizers. To build one, you simply need to instantiate the class by passing the tokenizer_func parameter. The parameter is a function that receives the entire sentence text as input and returns a list of token strings.

As an exercise, let's try to implement a custom tokenizer that splits the text into characters. This tokenizer will treat every character as a token:

```
```python
from flair.data import Token
from flair.tokenization import TokenizerWrapper

def char_splitter(sentence):
    return list(sentence)

char_tokenizer = TokenizerWrapper(char_splitter)
```
```

In the preceding code, we implemented the space_splitter function, which simply splits the sentence into a list of characters. Finally, we created the new tokenizer object by instantiating the TokenizerWrapper class with our new function.

Now, we can test our tokenizer by using it with a Sentence object and printing out the generated tokens:

```
```python
from flair.data import Sentence

text = "Good day."
sentence = Sentence(text, use_tokenizer=char_tokenizer)

for token in sentence:
    print(token)
```
```

The resulting script will print out the following:

```
```text
Token[0]: "G"
Token[1]: "o"
Token[2]: "o"
Token[3]: "d"
Token[4]: " "
Token[5]: "d"
```
```
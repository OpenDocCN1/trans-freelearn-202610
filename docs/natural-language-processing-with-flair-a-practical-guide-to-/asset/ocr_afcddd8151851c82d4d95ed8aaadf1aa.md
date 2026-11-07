# Download the color images

We also provide a PDF file that has color images of the screenshots and diagrams used in this book. You can download it here: https://static.packt-cdn.com/downloads/9781801072311_ColorImages.pdf.

# Conventions used

There are a number of text conventions used throughout this book.

Code in text: Indicates code words in the text, variable names, class names, object methods, and properties.

A block of code is set as follows:

```
from flair.data import Sentence
from flair.tokenization import SpaceTokenizer

tokenizer = SpaceTokenizer()
s = Sentence('Some nice text.', use_tokenizer=tokenizer)

print(s)
```

When we wish to draw your attention to a particular part of a code block, the relevant lines or items are set in bold:

```
from flair.data import Sentence

sentence = Sentence('A short sentence')
sentence.get_token(1).add_label('manual-pos', 'DT')

print(sentence)
```

Any command-line input or output is written as follows:

```
$ python3 --version
$ pip3 install flair==0.11
```

**Keyword**: Indicates a new term, an important name, or a concept of great significance; for example, **Hugging Face** transformer models.
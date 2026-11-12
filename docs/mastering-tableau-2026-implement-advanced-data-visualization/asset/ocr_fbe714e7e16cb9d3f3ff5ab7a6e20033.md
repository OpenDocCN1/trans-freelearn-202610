```
_arg1 = pd.to_datetime(
    _arg1, format='%Y-%m-%d')[0]

_arg2 = pd.to_datetime(
    _arg2, format='%Y-%m-%d')[0]

df = df.set_index('publish_date')
```

Then, we must convert the input into a readable format for our model.

```
scores = []
filtered_sentence = []
```

This part is to create two lists that we will fill in during the for loop.

```
in_range_df = df[df.index.isin(pd.date_range(_arg1, _arg2))]
```

Now we will filter the data to be between our start and end dates.

```
stop_words = set(stopwords.words('english'
'))
text = in_range_df['headline_text']
```

Luckily, English stop words are available at our disposal, and we are loading them in this step.

```
for row in range(0, len(in_range_df)):
 text = in_range_df['headline_text'][row]
 word_tokens = word_tokenize(text)
 for w in word_tokens:
 if w not in stop_words: filtered_sentence.append(w)
```

In this part of the code, we are converting the remaining headers into a token, which is needed to calculate the frequency, and finally, the non-stop-word words will be stored in the filtered_sentence list.

```
fdist = FreqDist(filtered_sentence)
```

By using one of the libraries we loaded in the beginning, we can now ask to retrieve the frequency for each word.

```
most_common_words = fdist.most_common(3)
return str(most_common_words)
```

And since we have the frequency now, we can load the most often used words.
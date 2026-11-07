```
if sentence.labels[0].value == "POSITIVE":
    return "buy"
elif sentence.labels[0].value == "NEGATIVE":
    return "sell"
else:
    return "hold"
```

This component is a class method called buy_or_sell(self, text). It receives text as input and, if the text is relevant to our company, performs sentiment analysis. If the text is deemed positive by the sentiment analysis classifier, we buy. If the text is negative, we sell. If the text isn't relevant to our company, we hold. It's important to note here that we completely disregard the classifier probability. Sometimes, the classifier will be 100% sure of its results, and sometimes, it'll be making a 51% guess. A good strategy will leverage this information and make use of certain probability thresholds.

> **Important Note**
> Make sure to add the preceding method to the FlairTrader class definition and not define it as a standalone piece of code.

Our trading strategy class is now complete!

We can now try it out by using another company name, such as Apple:

```
ft = FlairTrader("apple")
```

Let's see how our trading strategy performs:

```
print(ft.buy_or_sell("this year's apple harvest"))
```

The preceding code block prints out hold, which makes sense. The text is talking about apple as fruit and not Apple the company, meaning we gained no information about what trading decision to make.

Now, let's try an example with a positive sentiment:

```
print(ft.buy_or_sell("apple stock is doing great"))
```

The preceding code block, as expected, prints out buy.

We can now try a slightly less straightforward positive example:

```
print(ft.buy_or_sell("apple shows steady growth"))
```
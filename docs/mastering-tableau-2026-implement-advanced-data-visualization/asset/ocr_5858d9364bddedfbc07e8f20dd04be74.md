![](bbox=[0.18, 0.07, 0.82, 0.33])

# Figure 14.43: Words III

If you want to continue working on this, you could add the top as a model input parameter, as well as the sentiment score if you wanted to see the top for a specific sentiment only.

In this exercise, we were able to show that Tableau can analyze text data for its sentiment, something we have not seen before. To do so, we needed to connect Tableau with Python, which is possible by using the external service TabPy. Python can make use of libraries such as nltk .sentiment—which we used in this case. A thorough description of this package can be found here: https://www.nltk.org/api/nltk.sentiment.html. There are many other libraries for sentiment analysis, but nltk is used a lot for demonstration purposes and learning. Feel free to try other libraries as well!

Let's demystify some of the new code instances we included in this exercise:

| SCRIPT_REAL | This Tableau function calls the Python engine and returns a float value. |
|---|---|
| from nltk.sentiment import SentimentIntensityAnalyzer | In this part of the code, we need the Python library nltk, and from that library, we need the module sentiment, from which we will load the function SentimentIntensityAnalyzer. |
Tweet 139 is strange, though. It has a question mark before the word Obama, as if the name of a news article was being quoted, but there is no matching quote (or missing quote) at the end of the string. Was this supposed to be some other character? This might actually be a false positive too, or at least not enough of a true positive to actually make us fix it. While we are looking at the tweets closely, 224 also has an extra strange question mark in a place where it does not seem to belong.

If we are going to write a replace() function to change problematic question marks to single quotes, we will somehow need to write a regular expression that matches only the true positives and does not match any of the false positives. However, as this dataset is small, and there are only four true positives—or three if we decide 139 does not need to be cleaned—then we could just clean the true positives by hand. This is especially a good idea as we have a few questions about other possible issues such as the extra question mark in tweet 224.

In this case, as we only have three problem rows, it will be faster to simply run three small UPDATE commands on the data rather than attempting to craft the perfect regular expression. Here is the SQL query to take care of tweets 224 (first issue only), 613, and 2086:

```
UPDATE sentiment140 SET tweet_text = 'Life''s a bitch? and so is
  Dick Cheney. #p2 #bipart #tlot #tcot #hhrs #GOP #DNC
  http://is.gd/DjyQ' WHERE id = 224;
```

```
UPDATE sentiment140 SET tweet_text = 'Talk is Cheap: Bing that,
  I''ll stick with Google. http://bit.ly/XC3C8' WHERE id = 613;
```

```
UPDATE sentiment140 SET tweet_text = 'Stanford University''s
  Facebook Profile is One of the Most Popular Official University
  Pages - http://tinyurl.com/p5b3f1' WHERE id = 2086;
```

![](bbox=[0.2, 0.63, 0.25, 0.67])

> Note that we had to escape our single quotes in these update statements. In MySQL, the escape character is either the backslash or single quote itself. These examples show the single quote as the escape character.
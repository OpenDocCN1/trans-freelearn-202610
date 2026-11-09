# Moving this process into full (non-test) tables

Just like in *Chapter 9, Stack Overflow Project*, we made test tables so that we could develop our project in a stress-free environment with a manageable number of tweets to collect. When you are ready to collect the full list of tweets, be ready to spend some time doing so. Twitter's rate limits will kick in, and twarc will take a long time to run. Ed Summers indicates on this blog post that it will take about one week to run the Ferguson tweets: http://inkdroid.org/journal/2014/11/18/on-forgetting/. Of course, if you are careful, you will only have to run it once.

Another thing you could do to speed up the time it takes to hydrate the tweet IDs is to work as a team with someone else. You can divide the tweet ID file in half and each work on your portion of the tweets. During the data cleaning process, make sure you INSERT both into the same final database table.

Here are the steps we will follow to change our project to collect the full set of tweets rather than the 1,000-tweet sample:

+   1. Empty the `ferguson_tweets`, `ferguson_tweets_hashtags`, `ferguson_tweets_mentions`, and `ferguson_tweets_urls` tables as follows:
+   ```sql
+   TRUNCATE TABLE ferguson_tweets;
+   TRUNCATE TABLE ferguson_tweets_hashtags;
+   TRUNCATE TABLE ferguson_tweets_mentions;
+   TRUNCATE TABLE ferguson_tweets_urls;
+   ```
+   2. Run twarc on the full `ids.txt` file rather than the `ids_1000.txt` file as follows:
+   ```bash
+   twarc.py --consumer_key abcd --consumer_secret abcd --access_token abcd --access_token_secret abcd --hydrate ids.txt > tweets.json
+   ```
+   3. Re-run the `jsonTweetCleaner.py` Python script.

At this point, you will have a cleaned database full of tweets, hashtags, mentions, and URLs, ready for analysis and visualization. Since there are so many more rows now in each table, be aware that each of the visualization steps could take much longer to run, depending on what kind of queries you are running.
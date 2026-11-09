In the next step, we will need to choose one of these to download and begin to work with. Since the Ferguson tweets are the newest and the most complete of those three example data sets, I have designed the rest of this chapter around it. However, no matter what set you use, the concepts and basic procedures here will apply.

The basic question we want to ask in this chapter is quite simple: when people tweeted about Ferguson, which Internet domains did they tweet about most? Compared to our set of questions in Chapter 9, Stack Overflow Project, this is a very simple question, but the data collection process is somewhat different here, so a simple question is enough to motivate our data science dinner party.

# Step two – collecting the data

Unlike the small sentiment tweet archive that we studied in Chapter 7, RDBMS Cleaning Techniques, the newer tweet archives, like those mentioned, no longer contain the actual text of the tweets themselves. Twitter's Terms of Service (ToS) have changed as of 2014, and distributing other people's tweets is now a violation of this ToS. Instead, what you will find in a newer tweet archive is actually just the tweet identification (ID) numbers. Using these numbers, we will have to collect the actual tweets individually. At that point, we can store and analyze the tweets ourselves. Note that at any point during or after this process, we cannot redistribute the tweet text or their metadata, only the tweet identification numbers.

![](bbox=[0.2, 0.57, 0.25, 0.61])

> Even though it is inconvenient for the researcher who wants to collect the tweets, the stated reason for Twitter's change to their ToS is to honor the notion of copyright by the original person who posted the tweet. This is especially important in terms of deleting a tweet. If a tweet has been copied and redistributed all around the Web and stored in data files by third parties, the tweet cannot really be deleted. By asking that researchers copy only the tweet ID, Twitter attempts to protect the user's ability to delete his or her own content. A request for a deleted tweet ID will return no result.

# Download and extract the Ferguson file

The Ferguson-related tweets are available at the Internet Archive as a set of archived, compressed text files. Point your browser to https://archive.org/details/ ferguson-tweet-ids and download the 147 MB ZIP file.

We can extract the files as follows:

```
unzip ferguson-tweet-ids.zip
```
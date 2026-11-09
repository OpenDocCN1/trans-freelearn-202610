# Getting ready

To set up the examples in this chapter, we will be working with a popular dataset called **Sentiment140**. This dataset was created to help learn about the positive and negative sentiments in messages on Twitter. We are not really concerned with sentiment analysis in this book, but we are going to use this dataset to practice cleaning data after it has already been imported into a relational database.

To get started with the Sentiment140 dataset, you will need a MySQL server set up and ready to go, just like in the earlier Enron examples.

# Step one – download and examine Sentiment140

The version of the Sentiment140 data that we want to use is the original set of two files available directly from the Sentiment140 project at `http://help.sentiment140.com/for-students`. This ZIP file of tweets and their positive and negative polarity (or sentiment, on a scale of 0, 2, or 4) was created by some graduate students at Stanford University. Since this file was made publicly available, the original Sentiment140 files have been added by other websites and made available as part of many larger collections of tweets. For this chapter, we will use the original Sentiment140 text file, which is either available as a link from the preceding site or by following the precise path to `http://cs.stanford.edu/people/alecmgo/trainingandtestdata.zip`.

Download the ZIP file, extract it, and take a look at the two CSV files inside using your text editor. Right away, you will notice that one file has many more lines than the other, but both these files have the same number of columns in them. The data is comma-delimited, and each column has been enclosed in double quotes. The description of each column can be found on the `for-students` page linked in the preceding section.

# Step two – clean for database import

For our purposes—learning how to clean data—it will be sufficient to load the smaller of these files into a single MySQL database table. Everything we need to do to learn, we can accomplish with the smaller file, the one called `testdata.manual.2009.06.14.csv`.
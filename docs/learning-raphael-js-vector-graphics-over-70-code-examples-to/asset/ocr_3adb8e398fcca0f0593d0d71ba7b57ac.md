```
keyPerson.transform([ 'T', 0, bBox.height * 8 ]);
paper.text(
 bBox.width + 10,
 (bBox.height * 8) + 18,
 '= 50 million people'
).attr('text-anchor', 'start');
```

The position of this person path on our canvas is arbitrary. We place it at eight times the height of the paths bounding box so that it appears just below our six rows of people.

# # Tweets by time

There are approximately 3,000 tweets every second on average, meaning that over the space of 5 minutes there are about one million tweets created. In this section, we will create an interesting animated donut chart visualization that utilizes path arcs to illustrate the time taken for a particular number of tweets to be created:

![](bbox=[0.221, 0.428, 0.779, 0.807])
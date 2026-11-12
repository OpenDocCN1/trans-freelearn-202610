```
model.topicsMatrix()

topics_rdd = topics.rdd
topics_words = topics_rdd\
    .map(lambda row: row['termIndices'])\\
    .map(lambda idx_list: [vocab[idx] for idx in idx_list])\\
    .collect()
```

The following code snippet shows the topics:

```
for idx, topic in enumerate(topics_words) :
    print ("topic: ", idx)
    print ("-------------")
    for word in topic:
        print (word) # word
    print ("-------------")
```

```
-------
topic: 1
-------
armenian
armenians
jews
turkish
people
rights
jewish
israeli
right
government
-------
```

Figure 6.11 – Keywords contained in topic: 1

# Bisecting K-means

Bisecting K-means is a clustering algorithm that is a variant of the traditional K-means algorithm, designed to produce a hierarchical decomposition of the dataset. It combines aspects of K-means with a divisive (top-down) hierarchical clustering approach, making it distinct in its operation and application.
```
c('Would buy the product again', 'positive'),
c('Satisfied with response', 'positive'),
c('Helped me to choose between the two offers', 'positive'),
c('Definitely would recommend to a friend', 'positive'),
c('Terrible customer service', 'negative'),
c('not recommended', 'negative'),
c('Agent took too much time', 'negative'),
c('Waiting a long time for customer service', 'negative'),
c('Not satisfied with response', 'negative'),
c('Not satisfied at all', 'negative'),
c('Would not recommend', 'negative'),
c('Helpful customer support', 'positive'),
c('Great product support', 'positive'),
c('excellent product', 'positive')
```

```
)
# build Term document matrix
TDM.mat= as.matrix(create_matrix(verbatim[,1],language="english",
classifier = naiveBayes(TDM.mat[1:10,], as.factor(verbatim[1:
# test the model
predicted = predict(classifier, TDM.mat[11:15,]);
```

The table() function will tell you how the predictions did for the last five customer comments. Three of the five comments were classified correctly (60%). Is 60% considered good? That depends upon what you will be doing with your model. If you are able to identify negative comments, you might want to reach out to the customer via various channels and try to win the customer back. On the other hand, positive comments imply that you are doing right by the customer and maybe you should think about offering good customer discounts.

```
table(verbatim[11:15, 2], predicted)
> table(verbatim[11:15, 2], predicted)
          predicted
          negative positive
negative    1        1
positive    1        2
```
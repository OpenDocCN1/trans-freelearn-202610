| Country | Label encoder | Binary | Col1 | Col2 | Col3 |
|---|---|---|---|---|---|
| India | 1 | 001 | 0 | 0 | 1 |
| Canada | 2 | 010 | 0 | 1 | 0 |
| Brazil | 3 | 011 | 1 | 1 | 1 |
| Australia | 4 | 100 | 1 | 0 | 0 |
| India | 1 | 001 | 0 | 0 | 1 |

Figure 3.5 – Binary encoding in action

As we can see, we now have three columns (Col1, Col2, and Col3) instead of four.

# Transforming ordinal features

Ordinal features have a very specific characteristic: they have an order. Because they have this quality, it does not make sense to apply one-hot encoding to them; if you do so, you will lose the magnitude of order of your feature.

The most common transformation for this type of variable is known as ordinal encoding. An ordinal encoder will associate a number with each distinct label of your variable, just like a label encoder does, but this time, it will respect the order of each category. The following table shows how an ordinal encoder works:

| Education | Ordinal encoding |
|---|---|
| Trainee | 1 |
| Jr. Data Analyst | 2 |
| Sr Data Analyst | 3 |
| Chief Data Scientist | 4 |

Figure 3.6 – Ordinal encoding in action

We can now pass the encoded variable to ML models and they will be able to handle this variable properly, with no need to apply one-hot encoding transformations. This time, comparisons such as "Sr Data Analyst is greater than Jr. Data Analyst" make total sense.

# Avoiding confusion in our train and test datasets

Do not forget the following statement: encoders are fitted on training data and transformed on test and production data. This is how your ML pipeline should work.
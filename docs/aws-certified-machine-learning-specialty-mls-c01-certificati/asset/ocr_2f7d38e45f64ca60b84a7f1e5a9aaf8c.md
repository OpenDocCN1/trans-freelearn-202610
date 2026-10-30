| Country | Label encoding |
|---|---|
| India | 1 |
| Canada | 2 |
| Brazil | 3 |
| Australia | 4 |
| India | 1 |

Figure 3.3 – Label encoder in action

A label encoder will always ensure that a unique number is associated with each distinct label. In the preceding table, although "India" appears twice, the same number was assigned to it.

You now have a numerical representation of each country, but this does not mean you can use that numerical representation in your models! In this particular case, we are transforming a nominal feature, which does not have an order.

According to the preceding table, if we pass the encoded version of the country variable to a model, it will make assumptions such as "Brazil (3) is greater than Canada (2)", which does not make any sense.

A possible solution for that scenario is applying another type of transformation on top of "country": one-hot encoding. This transformation will represent all the categories from the original feature as individual features (also known as dummy variables), which will store the "presence or absence" of each category. The following table is transforming the same information we looked at in the preceding table, but this time it's applying one-hot encoding:

| Country | India | Canada | Brazil | Australia |
|---|---|---|---|---|
| India | 1 | 0 | 0 | 0 |
| Canada | 0 | 1 | 0 | 0 |
| Brazil | 0 | 0 | 1 | 0 |
| Australia | 0 | 0 | 0 | 1 |
| India | 1 | 0 | 0 | 0 |

Figure 3.4 – One-hot encoding in action

We can now use the one-hot encoded version of the country variable as a feature of a ML model. However, your work as a skeptical data scientist is never done, and your critical thinking ability will be tested in the AWS Machine Learning Specialty exam.
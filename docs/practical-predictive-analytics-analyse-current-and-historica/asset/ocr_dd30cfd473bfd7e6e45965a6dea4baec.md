# # What is market basket analysis?

If you have survived the last chapter, you will now be introduced to the world of **market basket analysis (MBA)**. Market basket analysis (also sometimes called **affinity analysis**), is a predictive analytics technique that is used heavily in the retail industry in order to identify baskets of items that are purchased together. The typical use case for this is the supermarket shopping cart in which a shopper would typically purchase an assortment of items such as milk, bread, cheese, and so on, and the algorithm will predict how purchasing certain items together will affect the purchase of other items. It is one of those methods that retailers use to know to start sending you coupons and emails for things that you didn't know you needed!

One often quoted example of MBA is the relationship between diapers and beer:

> > "One super market chain discovered in its analysis that customers that bought diapers often bought beer as well, have put the diapers close to beer coolers, and their sales increased dramatically" Ã Â Â Â Â Â Â Ã Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â -
> http://en.wikipedia.org/wiki/Market_basket

However, it is not only restricted to the retail industry. MBA can be used in the insurance industry to look at the various products that an insured person currently has, such as a car, home, and so on, and suggest other possible products such as life, disability, or investment products.

MBA is generally considered an unsupervised learning algorithm, in that target variables are usually not specified. However, as you will see later it is possible to refine the association rules, so that specific items can be specified as target variables.

MBA is also considered a type of recommender engine in which purchases of a set of items imply the purchase of others. Certainly, MBA and other recommender engines can share the same types of input data. However, MBA
If the ratings or reviews are explicitly given for a set of items or products, it makes sense to apply user based collaborative filtering. If some of the ratings for few items are missing still the data can be imputed, once the missing ratings predicted, the user similarity can be computed and hence recommendation can be generated. For user-based collaborative filtering, the data would look as follows:

| User | Item1 | Item2 | Item3 | Item4 | Item5 | Item6 |
|---|---:|---:|---:|---:|---:|---:|
| user1 | -7.82 | 8.79 | -9.66 | -8.16 | -7.52 | -8.5 |
| user2 | 4.08 | -0.29 | 6.36 | 4.37 | -2.38 | -9.66 |
| user3 |  |  |  |  | 9.03 | 9.27 |
| user4 |  | 8.35 |  |  | 1.8 | 8.16 |
| user5 | 8.5 | 4.61 | -4.17 | -5.39 | 1.36 | 1.6 |
| user6 | -6.17 | -3.54 | 0.44 | -8.5 | -7.09 | -4.32 |
| user7 |  |  |  |  | 8.59 | -9.85 |
| user8 | 6.84 | 3.16 | 9.17 | -6.21 | -8.16 | -1.7 |
| user9 | -3.79 | -3.54 | -9.42 | -6.89 | -8.74 | -0.29 |
| user10 | 3.01 | 5.15 | 5.15 | 3.01 | 6.41 | 5.15 |

Table 1: User-based item sample dataset

If the binary matrix is given as an input, where the levels represent whether the product is bought or not, then it is recommended to apply item-based collaborative filtering. The sample dataset is given next:

| User | Item1 | Item2 | Item3 | Item4 | Item5 | Item6 |
|---|---:|---:|---:|---:|---:|---:|
| user1 | 1 | 1 | 1 | 1 | 1 | 0 |
| user2 | 1 | 1 | 0 | 0 | 1 |  |
| user3 | 0 | 0 |  | 1 | 0 | 0 |
| user4 | 0 | 1 | 1 |  | 1 | 1 |
| user5 | 1 | 0 |  | 1 | 0 |  |
| user6 | 0 | 0 | 1 |  |  | 0 |
| user7 |  | 1 | 0 | 0 | 1 | 1 |
| user8 | 1 | 0 | 1 | 1 | 0 |  |
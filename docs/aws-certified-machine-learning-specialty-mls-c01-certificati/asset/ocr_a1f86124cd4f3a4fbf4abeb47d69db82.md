|  | Dim 1 | Dim 2 | Dim 3 | Dim 4 | Dim 5 |
|---|---|---|---|---|---|
| King | 0.22 | 0.76 | 0.77 | 0.44 | 0.33 |
| Queen | 0.98 | 0.09 | 0.67 | 0.89 | 0.56 |
| Live | 0.13 | 0.99 | 0.88 | 0.01 | 0.55 |
| Castle | 0.01 | 0.89 | 0.34 | 0.02 | 0.90 |

![](bbox=[0.45, 0.21, 0.52, 0.27])

|  | PC 1 | PC2 |
|---|---|---|
| King | 0.9 | 0.3 |
| Queen | 0.8 | 0.5 |
| Live | 0.33 | 0.7 |
| Castle | 0.28 | 0.7 |

![](bbox=[0.45, 0.43, 0.52, 0.49])

![](bbox=[0.32, 0.48, 0.65, 0.66])

Figure 3.25 – Plotting words

In case you are wondering how we came up with the dimensions shown in Figure 3.24, let's dive into it. Again, there are different methods to do this, but let's look at one of the most popular, which uses a co-occurrence matrix with a fixed context window.

First, we have to come up with some logic to represent each word, keeping in mind that we also have to take their context into consideration. To solve the context requirement, we will define a fixed context window, which is going to be responsible for specifying how many words will be grouped together for context learning. For instance, let's set this fixed context window to 2.
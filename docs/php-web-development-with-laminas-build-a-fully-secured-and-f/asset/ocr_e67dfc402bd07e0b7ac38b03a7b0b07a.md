![](bbox=[0.165, 0.095, 0.81, 0.545])

# Figure 6.3 – Creating the ProductUpdatingContext class

We need to add the new context class to the behat .yml file in the features folder. The section contexts must be like this:

```
contexts:
  - ProductAdditionContext
  - ProductUpdatingContext
```

If you execute Behat run now, the output will end with the following:

```
[0] None
[1] ProductAdditionContext
[2] ProductUpdatingContext
```

Right, the new context class is appearing to be selected. But if you look at what appears before these options, you will notice that two scenarios are presented, Product Addition and Product Updating. When we don't tell Behat which scenarios we want, Behat brings all of them.
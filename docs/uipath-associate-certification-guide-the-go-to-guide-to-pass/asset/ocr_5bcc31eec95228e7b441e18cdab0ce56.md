![](bbox=[0.25, 0.09, 0.7, 0.22])

# Figure 14.31 – Capturing the date from the invoice

11. Let's look at the selector of this Get Text activity. In this case, the date mentioned in the invoice is captured by the selector. Here is the part of the selector:

```
<ctrl name=' Date: 16/07/2021 ' role='text' />
```

As we want it to be dynamic to work with any other date provided, we will use the dynamic selector concepts again. Replace the 16/07/2021 part with * and click on the Validate button. The Validate button turns green, which confirms that the new selector is correct. Your new selector code should look as follows:

```
<ctrl name=' Date: * ' role='text' />
```

* will make sure that it works for any date provided in the invoice:

![](bbox=[0.2, 0.48, 0.76, 0.85])

# Figure 14.32 – The Selector Editor window of the Get Text activity for the invoice date
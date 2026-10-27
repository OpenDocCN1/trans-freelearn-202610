As was the case for the position fact, we do not have special requirements now, so we are just publishing everything as it is in the REF model.

As usual, to deploy the model, run the following command:

```
dbt build -s DIM_SECURITY
```

And if you want to be sure that everything is deployed in the most recent form, run the following:

```
dbt build
```

After running one of the commands, you will have the two tables in the data mart, FACT_POSITION and DIM_SECURITY. You are now ready to start doing your analysis.

# Exercise – adding a few more hand-maintained dimensions

In this chapter, we have shown you how to go from the source data, including creating a CSV when the data is not available elsewhere, through the storage and refined layers to produce data marts that you can use to power your analysis.

![](bbox=[0.13, 0.47, 0.88, 0.6])

Figure 7.10: The layers of the Pragmatic Data Platform

Here, we propose you add a few dimensions to the project to provide more useful info on the fact that we have loaded.

For each dimension, you will create the dbt models for all the layers of the data platform and you will start to see how you keep applying the same patterns over and over.

Potential candidates and columns for each are as follows:

+   - **Exchange**: A dimension describing the exchanges where securities are exchanged, with code, name, country, city, trading hours, and time zone
+   - **Account**: A dimension describing the accounts holding the positions with code, nickname, primary beneficiary name, all beneficiaries’ names, accounting currency, broker name or broker code
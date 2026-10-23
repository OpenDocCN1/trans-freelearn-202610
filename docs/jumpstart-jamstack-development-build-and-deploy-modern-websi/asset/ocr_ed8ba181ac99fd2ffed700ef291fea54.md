Two of the main tools – the **Dashboard** and **Desk** are automatically installed. To be able to try out GROQ with actual queries against a dataset, we will install a third tool, called **Vision**. Vision gives us a way to type in queries and instantly view the resulting dataset, optionally filtered and sorted. This will help us use GROQ inside of code.

# Installing Vision

To install Vision, take the following steps and use the terminal command line:

+   1. Inside the `studio` folder, type the following command:

```
sanity install @sanity/vision
```

Upon successful installation, the **Vision** icon should be visible together with the other tools in your local Sanity Studio application:

![](bbox=[0.27, 0.382, 0.696, 0.427])

Figure 5.1 – The top navigation

+   2. Click on the **Vision** icon and a screen will appear that will allow the user to execute GROQ queries:

![](bbox=[0.117, 0.519, 0.851, 0.757])

Figure 5.2 – The Vision screen for GROQ queries

In the top navigation, the **DATASET** selector allows the user to select which dataset to use. The light-blue **Run query** button will perform the query. The **Listen** button will activate a mode where changes in the documents from the filter part of your GROQ query are output in real time in the **RESULT** section.
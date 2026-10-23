Click on the Open in Data Visualizer button to display the following chart:

# Data Visualizer

![](bbox=[0.129, 0.168, 0.838, 0.531])

Figure 8.5: Data distribution

We can see that the index contains more than 33,000 emails, with 2 labels—spam and ham—where just over half of the emails are spam and the rest are ham. We are lucky to have such a straightforward dataset and reference to use with ELSER, as the heavy lifting has already been done for us from a data structuring perspective, and the data import functionality allows us to quickly onboard the data in Elastic. Next, we are going to see ELSER in action on top of this data and start detecting spam inside the dataset to verify whether we land close to the labeling that has been prepared already.

# Seeing ELSER in action

In this part, we are going to walk you through how easy it is to get started with ELSER and see some significant results right out of the box. The first part will be to go through the required hardware. Then, we will look at preparing the index; finally, we are going to fire a couple of queries to illustrate the power of ELSER.
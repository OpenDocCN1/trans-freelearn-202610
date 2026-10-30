# Using AWS's built-in algorithm for PCA

In AWS, PCA works in two different modes:

+   - **Regular:** For datasets with a moderate number of observations and features
+   - **Randomized:** For datasets with a large number of observations and features

The difference is that, in randomized mode, it is used an approximation algorithm.

Of course, the main hyperparameter of PCA is the number of components that you want to extract, known as `num_components`.

# IP Insights

IP Insights is an unsupervised algorithm that's used for pattern recognition. Essentially, it learns the usage pattern of IPv4 addresses.

The modus operandi of this algorithm is very intuitive: it is trained on top of pairs of events in the format of entity and IPv4 address so that it can understand the pattern of each entity that it was trained on.

> **Important note**
> For instance, we can understand "entity" as user IDs or account numbers.

Then, to make predictions, it receives a pair of events with the same data structure (entity, IPv4 address) and returns an anomaly score for that particular IP address regarding the input entity.

> **Important note**
> This anomaly score that's returned by IP Insight infers how anomalous the pattern of the event is.

We might come across many applications with IP Insights. For example, you can create an IP Insights model that was trained on top of your application login events (this is your entity). You should be able to expose this model through an API endpoint to make predictions in real time.

Then, during the authentication process of your application, you could call your endpoint and pass the IP address that is trying to log in. If you got a high score (meaning this pattern of logging in looks anomalous), you can request extra information before authorizing access (even if the password was right).
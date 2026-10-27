If you choose to transform your time series into images, you can also leverage Amazon Rekognition Custom Labels for classification or Amazon Lookout for Vision to perform anomaly detection.

Once you have an existing database of activities, you can also leverage an indexing approach: in this case, building a symbolic representation of your time series will allow you to use it as an embedding to query similar time series in a database, or even past segments of the same time series if you want to discover potential recurring motifs.

# Predictive quality

Imagine that you have a process that ends up with a product or service with a quality that can vary depending on how well the process was executed. This is typically what can happen on a manufacturing production line where equipment sensors, process data, and other tabular characteristics can be measured and matched to the actual quality of the finished goods.

You can then use all these time series to build a predictive model that tries to predict if the current batch of products will achieve the appropriate quality grade or if it will have to be reworked or thrown away as waste.

**Recurring neural networks (RNNs)** are traditionally what is built to address this use case. Depending on how you shape your available dataset, you might however be able to use either Amazon Forecast (using the predicted quality or grade of the product as the main time series to predict and all the other available data as related time series) or Amazon Lookout for Equipment (by considering bad product quality as anomalies for which you want to get as much forewarning as possible).

# Setpoint optimization

In process industries (industries that transform raw materials into finished goods such as shampoo, an aluminum coil, or a piece of furniture), setpoints are the target value of a process variable. Imagine that you need to keep the temperature of a fluid at 50°C; then, your setpoint is 50°C. The actual value measured by the process might be different and the objective of process control systems is to ensure that the process value reaches and stays at the desired setpoint.

In such a situation, you can leverage the time series data of your process to optimize an outcome: for instance, the quantity of waste generated, the energy or water used, or the change to generate a higher grade of product from a quality standpoint (see the previous predictive quality use case for more details). Based on the desired outcome, you can then use an ML approach to recommend setpoints that will ensure you reach your objectives.
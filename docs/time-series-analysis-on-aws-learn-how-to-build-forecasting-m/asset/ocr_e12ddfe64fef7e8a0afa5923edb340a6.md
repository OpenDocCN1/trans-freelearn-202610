These defects might come from the environmental conditions (hygrometry of the factory; external temperature; rainfall; raw material supplier; bad programming sequence entered by an operator). This kind of problem might be better framed as an anomaly detection problem: in this case, a service such as Amazon Lookout for Equipment (which will be our focus in Part 3, Detecting Abnormal Behavior in Multivariate Time Series with Amazon Lookout for Equipment, of this book) might be better suited to provide the insights you are looking for.

After deciding that forecasting is indeed what you want to leverage to solve your problem, there are several questions you need to address in the early stages of your forecasting project, as follows:

+   - What would you like to forecast?
+   - What is the frequency of the available data (hourly, daily, monthly...)?
+   - What is the desired forecast horizon?
+   - How frequently do you need to forecast?
+   - What is the expected output of the forecasting exercise?
+   - How will you evaluate the accuracy of your forecasts?
+   - How will the forecast outputs be used further down the road?

Moreover, a key task is obviously to collect, assemble, and prepare the historical data needed to build your forecasting model. You will also need to identify whether related data known at the same time can lend some statistical power to your model to improve your forecast accuracy; as time series forecasting practitioners, you might have used approaches such as Granger predictive causality tests to validate whether a given time series can be used to forecast another one.

Collecting and preparing this related data will be an additional step before you can build your model. You might also need to collect and prepare non-time-related relationships between your time series (such as hierarchies, where some locations are in the same region, or some items are in the same product groups, or some items share the same color).
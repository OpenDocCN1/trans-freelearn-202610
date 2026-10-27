
Depending on the dynamics and complexity of your data, automated labeling can actually require additional verification to validate the quality of the generated labels. Automated labeling can be used to kick-start a manual labeling process with prelabelled data to be confirmed by a human operator.

## Selecting an analysis approach

By now, we have met the different families of time series-related datasets, and we have listed key potential challenges you may encounter when processing them. In this section, we will describe the different approaches we can take to analyze time series data.

## Using raw time series data

Of course, the first and main way to perform time series analysis is to use the raw time sequences themselves, without any drastic transformation. The main benefit of this approach is the limited amount of preprocessing work you have to perform on your time series datasets before starting your actual analysis.

What can we do with these raw time series? We can leverage *Amazon SageMaker* and leverage the following built-in algorithms:

- **Random Cut Forest**: This algorithm is a robust cousin from the **scikit-learn Isolation Forest algorithm** and can compute an anomaly score for each of your data points. It considers each time series as a univariate one and cannot learn any relationship or global behavior from multiple or multivariate time series.
- **DeepAR**: This probabilistic forecasting algorithm is a great fit if you have hundreds of different time series that you can leverage to predict their future values. Not only does this algorithm provide you with point prediction (several single values over a certain forecast horizon), but it can also predict different quantiles, making it easier to answer questions such as: *Given a confidence level of 80% (for instance), what boundary values will my time series take in the near future?*

In addition to these built-in algorithms, you can of course bring your favorite packages to Amazon SageMaker to perform various tasks such as forecasting (by bringing in `Prophet` and `NeuralProphet` libraries) or change-point detection (with the `ruptures` Python package, for instance).

The following three AWS services we dedicate a part to in this book also assume that you will provide raw time series datasets as inputs:

- **Amazon Forecast**: To perform time series forecasting
- **Amazon Lookout for Equipment**: To deliver anomalous event forewarning usable to improve predictive maintenance practices
- **Amazon Lookout for Metrics**: To detect anomalies and help in **root-cause analysis (RCA)**

Other open source packages and AWS services can process transformed time series datasets. Let's now have a look at the different transformations you can apply to your time series.

## Summarizing time series into tabular datasets

The first class of transformation you can apply to a time series dataset is to compact it by replacing each time series with some metrics that characterize it. You can do this manually by taking the mean and median of your time series or computing standard deviation. The key objective in this approach is to simplify the analysis of a time series by transforming a series of data points into one or several metrics. Once summarized, your insights can be used to support your EDA or build classification pipelines when new incoming time series sequences are available.

You can perform this summarization manually if you know which features you would like to engineer, or you can leverage Python packages such as the following:

- `tsfresh`: (http://tsfresh.com)
- `hctsa`: (https://github.com/benfulcher/hctsa)
- `featuretools`: (https://www.featuretools.com)
- `Cesium`: (http://cesium-ml.org)
- `FATS`: (http://isadoranun.github.io/tsfeat)

These libraries can take multiple time series as input and compute hundreds, if not thousands, of features for each of them. Once you have a traditional tabular dataset, you can apply the usual ML techniques to perform dimension reduction, uncover feature importance, or run classification and clustering techniques.

By using Amazon SageMaker, you can access well-known algorithms that have been adapted to benefit from the scalability offered by the cloud. In some cases, they have been rewritten from scratch to enable linear scalability that makes them practical on huge datasets! Here are some of them:

- **Principal Component Analysis (PCA)**: This algorithm can be used to perform dimensionality reductions on the obtained dataset (https://docs.aws.amazon.com/sagemaker/latest/dg/pca.html).
- **K-Means**: This is an unsupervised algorithm where each row will correspond to a single time series and each computed attribute will be used to compute a similarity between the time series (https://docs.aws.amazon.com/sagemaker/latest/dg/k-means.html).
- **K-Nearest Neighbors (KNN)**: This is another algorithm that benefits from a scalable implementation. Once your time series has been transformed into a tabular dataset, you can frame your problem either as a classification or a regression problem and leverage KNN (https://docs.aws.amazon.com/sagemaker/latest/dg/k-nearest-neighbors.html).
- **XGBoost**: This is the Swiss Army knife of many data scientists and is a well-known algorithm to perform supervised classification and regression on tabular datasets (https://docs.aws.amazon.com/sagemaker/latest/dg/xgboost.html).

You can also leverage Amazon SageMaker and bring your custom preprocessing and modeling approaches if the built-in algorithms do not fit your purpose. Interesting approaches could be more generalizable dimension reduction techniques (such as **Uniform Manifold Approximation and Projection (UMAP)** or **t-distributed stochastic neighbor embedding (t-SNE)** and other clustering techniques such as **Hierarchical Density-Based Spatial Clustering of Applications with Noise (HDBSCAN)**). Once you have a tabular dataset, you can also apply and neural network-based approaches for both classification and regression.

## Using imaging techniques

A time series can also be transformed into an image; this allows you to leverage the very rich field of computer vision approaches and architectures. You can leverage the pyts Python package to apply the transformation described in this chapter (https://pyts.readthedocs.io) and, more particularly, the pyts.image submodule.

Once your time series is transformed into an image, you can leverage many techniques to learn about it, such as the following:

- Use **convolutional neural networks** (CNN) architectures to learn valuable features and build such custom models on Amazon SageMaker or use its built-in **image classification** algorithm.
- Leverage **Amazon Rekognition Custom Labels** to rapidly build models that can rapidly classify different behaviors that would be highlighted by the image transformation process.
- Use **Amazon Lookout for Vision** to perform anomaly detection and classify which images are representative of an underlying issue captured in the time series.

Let's now have a look at the different transformations we can apply to our time series and what they can let you capture about their underlying dynamics. We will deep dive into the following:

- **Recurrence plots**
- **Gramian angular fields**
- **Markov transition fields (MTF)**
- **Network graphs**

## Recurrence plots

A recurrence is a time at which a time series returns back to a value it has visited before. A recurrence plot illustrates the collection of pairs of timestamps at which the time series is at the same place—that is, the same value. Each point can take a binary value of 0 or 1. A recurrence plot helps in understanding the dynamics of a time series and such recurrence is closely related to dynamic non-linear systems. An example of recurrence plots being used can be seen here:

Then, the Gram matrix is computed as follows:

```
$$G=\begin{bmatrix}\cos(\varphi_1+\varphi_1)&\cos(\varphi_1+\varphi_2)&\cdots&\cos(\varphi_1+\varphi_n)\\\cos(\varphi_2+\varphi_1)&\cos(\varphi_2+\varphi_2)&\cdots&\cos(\varphi_2+\varphi_n)\\\vdots&\vdots&\ddots&\vdots\\\cos(\varphi_n+\varphi_1)&\cos(\varphi_n+\varphi_2)&\cdots&\cos(\varphi_n+\varphi_n)\end{bmatrix}$$
```

Let's plot the Gramian angular fields of the same three time series as before, as follows:

![](img/e4ee48104c730845e796d8c469dfeecc_56_0.png)

Compared to the previous representation (the recurrence plot), the Gramian angular fields are less prone to generate white noise for heavily period signals (such as the pump signal below). Moreover, each point takes a real value and is not limited to 0 and 1 (hence the colored pictures instead of the grayscale ones that recurrence plots can produce).

## MTF

MTF is a visualization technique to highlight the behavior of time series. To build an MTF, you can use the `pyts.image` module. Under the hood, this is the transformation that is applied to your time series:

- Discretization of the time series along with the different values it can take.
- Build a Markov transition matrix.
- Compute the transition probabilities from one timestamp to all the others.
- Reorganize the transition probabilities into an MTF.
- Compute an aggregated MTF.

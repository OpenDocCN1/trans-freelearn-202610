# Training a CNN-QR predictor with HPO

In this section, we are going to train a second predictor, using the winning algorithm found in *Chapter 4, Training a Predictor with AutoML*. As a reminder, the winning algorithm was CNN-QR and we achieved the following performance:

| Evaluation type | Test window start | Test window end | Items | wQL[0.1] | wQL[0.5] | wQL[0.9] | WAPE | RMSE |
|---|---|---|---:|---:|---:|---:|---:|---:|
| Backtest window | Sat, 01 Jun 2013 00:00:00 GMT | Mon, 01 Jul 2013 00:00:00 GMT | 3573 | 0.0993 | 0.2254 | 0.1183 | 0.2254 | 3.2904 |

Figure 5.3 – Winning algorithm performance with AutoML

To train a new predictor, log in to the AWS console, navigate to the Amazon Forecast console, and click on the **View dataset groups** button. There, you can click on the name of the dataset group you created in *Chapter 3, Creating a Project and Ingesting your Data*. Check out this chapter if you need a reminder on how to navigate through the Amazon Forecast console. You should now be on the dashboard of your dataset group, where you will click on the **Train predictor** button, as illustrated in the following screenshot:

![](bbox=[0.146, 0.46, 0.818, 0.828])

Figure 5.4 – Dataset group initial dashboard
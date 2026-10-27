# Configuring the solution

The *Improving Forecast Accuracy with Machine Learning* solution is configured with a YAML file that you will need to upload to the S3 bucket you took note of earlier (*Step 9 of the Solutions deployment section*). The file you will use will contain an introduction section configured as follows:

```
Household_energy_consumption:
  DatasetGroup:
    Domain: CUSTOM
```

You then configure the different datasets to be used. Here is the section for the target time series dataset:

```
Datasets:
  - Domain: CUSTOM
    DatasetType: TARGET_TIME_SERIES
    DataFrequency: D
    TimestampFormat: yyyy-MM-dd
    Schema:
      Attributes:
        - AttributeName: item_id
          AttributeType: string
```

Next, you will describe how the predictor will be trained:

```
Predictor:
  MaxAge: 604800
  PerformAutoML: False
  ForecastHorizon: 30
  AlgorithmArn: arn:aws:forecast:::algorithm/CNN-QR
  FeaturizationConfig:
    ForecastFrequency: D
  TrainingParameters:
    epochs: "100"
    context_length: "240"
    use_item_metadata: "ALL"
    use_related_data: "FORWARD_LOOKING"
```
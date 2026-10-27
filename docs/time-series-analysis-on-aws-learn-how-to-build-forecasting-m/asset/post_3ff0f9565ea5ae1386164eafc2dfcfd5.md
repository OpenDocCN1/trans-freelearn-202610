
## Time Series Analysis
### on AWS

Learn how to build forecasting models and detect anomalies in your time series data

Michaël Hoarau

![](img/e4ee48104c730845e796d8c469dfeecc_0_0.png)

## Time Series Analysis on AWS

Learn how to build forecasting models and detect anomalies in your time series data

Michaël Hoarau

![](img/e4ee48104c730845e796d8c469dfeecc_1_0.png)

BIRMINGHAM—MUMBAI

## Time Series Analysis on AWS

Copyright © 2022 Packt Publishing

*All rights reserved.* No part of this book may be reproduced, stored in a retrieval system, or transmitted in any form or by any means, without the prior written permission of the publisher, except in the case of brief quotations embedded in critical articles or reviews.

Every effort has been made in the preparation of this book to ensure the accuracy of the information presented. However, the information contained in this book is sold without warranty, either express or implied. Neither the author, nor Packt Publishing or its dealers and distributors, will be held liable for any damages caused or alleged to have been caused directly or indirectly by this book.

Packt Publishing has endeavored to provide trademark information about all of the companies and products mentioned in this book by the appropriate use of capitals. However, Packt Publishing cannot guarantee the accuracy of this information.

**Publishing Product Manager:** Reshma Raman  
**Senior Editor:** Roshan Kumar, David Sugarman  
**Content Development Editor:** Tazeen Shaikh  
**Technical Editor:** Sonam Pandey  
**Copy Editor:** Safis Editing  
**Project Coordinator:** Aparna Ravikumar Nair  
**Proofreader:** Safis Editing  
**Indexer:** Subalakshmi Govindhan  
**Production Designer:** Jyoti Chauhan  
**Marketing Coordinator:** Priyanka Mhatre

First published: February 2022

Production reference: 2240222

Published by Packt Publishing Ltd.  
Livery Place  
35 Livery Street  
Birmingham  
B3 2PB, UK.

ISBN 978-1-80181-684-7

www.packt.com

> To my parents, Roseline and Philippe, for their life-long dedication to offering the best learning environment I could hope for. To my wife, Nathalie, for her love, support, and inspiration. To my son, as he starts his journey to be a great creative thinker.
> 
> — Michaël Hoarau

## Contributors

## About the author

**Michaël Hoarau** is an AI/ML specialist **solutions architect (SA)** working at **Amazon Web Services (AWS)**. He is an AWS Certified Associate SA. He previously worked as an AI/ML specialist SA at AWS and the EMEA head of data science at GE Digital. He has experience in building product quality prediction systems for multiple industries. He has used forecasting techniques to build virtual sensors for industrial production lines. He has also helped multiple customers build forecasting and anomaly detection systems to increase their business efficiency.

> *I want to thank the people who have been close to me and supported me through the many evenings and weekends spent writing this book, especially my wife, Nathalie, and my son, Valentin.*

## Table of Contents

## Preface

## Section 1: Analyzing Time Series and Delivering Highly Accurate Forecasts with Amazon Forecast

### 1 An Overview of Time Series Analysis

| Content | Page | Content | Page |
|---|---:|---|---:|
| Technical requirements | 4 | Data quality | 20 |
| What is a time series dataset? | 4 | Visualization challenges | 22 |
| Recognizing the different families of time series | 5 | Behavioral challenges | 26 |
| Univariate time series data | 6 | Missing insights and context | 29 |
| Continuous multivariate data | 6 | Selecting an analysis approach | 31 |
| Event-based multivariate data | 8 | Using raw time series data | 31 |
| Multiple time series data | 9 | Summarizing time series into tabular datasets | 32 |
| Adding context to time series data | 10 | Using imaging techniques | 34 |
| Labels | 11 | Symbolic transformations | 39 |
| Related time series | 13 | Typical time series use cases | 42 |
| Metadata | 13 | Virtual sensors | 43 |
| Learning about common time series challenges | 14 | Activity detection | 43 |
| Technical challenges | 15 | Predictive quality | 44 |
|  |  | Setpoint optimization | 44 |
|  |  | Summary | 45 |

### 2 An Overview of Amazon Forecast

- Technical requirements — 48
- What kinds of problems can we solve with forecasting? — 49
- Framing your forecasting problem — 51
- What is Amazon Forecast? — 54
- How does Amazon Forecast work? — 56
- Amazon Forecast workflow overview — 58
- Pricing — 59
- Choosing the right applications — 60
- Latency requirements — 61
- Dataset requirements — 61
- Use-case requirements — 62
- Summary — 63

### 3 Creating a Project and Ingesting Your Data

- Technical requirements — 66
- Understanding the components of a dataset group — 66
- Target time series — 67
- Related time series — 67
- Item metadata — 68
- Preparing a dataset for forecasting purposes — 69
- Preparing the raw dataset (optional) — 69
- Uploading your data to Amazon S3 for storage — 76
- Authorizing Amazon Forecast to access your S3 bucket (optional) — 82
- Creating an Amazon Forecast dataset group — 84
- Ingesting data in Amazon Forecast — 86
- Ingesting your target time series dataset — 86
- Ingesting related time series — 94
- Ingesting metadata — 96
- What's happening behind the scenes? — 98
- Summary — 98

### 4 Training a Predictor with AutoML

- Technical requirements — 102
- Using your datasets to train a predictor — 102
- How Amazon Forecast leverages automated machine learning — 108
- Understanding the predictor evaluation dashboard — 109
    - Predictor overview 109
    - Predictor metrics 110
    - Exporting and visualizing your predictor backtest results 112
    - What is backtesting? 112
    - Exporting backtest results 114
    - Backtest predictions overview 116
    - Backtest accuracy overview 119
    - Summary 121

### 5 Customizing Your Predictor Training

- Technical requirements 124
- Choosing an algorithm and configuring the training parameters 124
    - ETS 126
    - ARIMA 129
    - NPTS 131
    - Prophet 133
    - DeepAR+ 134
    - CNN-QR 136
    - When should you select an algorithm? 137
- Leveraging HPO 139
    - What is HPO? 140
    - Training a CNN-QR predictor with HPO 141
    - Introducing tunable hyperparameters with HPO 145
- Reinforcing your backtesting strategy 146
- Including holiday and weather data 149
    - Enabling the Holidays feature 150
    - Enabling the Weather index 150
- Implementing featurization techniques 154
    - Configuring featurization parameters 155
    - Introducing featurization parameter values 157
- Customizing quantiles to suit your business needs 158
    - Configuring your forecast types 159
    - Choosing forecast types 160
- Summary 161

### 6 Generating New Forecasts

- Technical requirements 164
- Generating a forecast 164
    - Creating your first forecast 165
    - Generating new subsequent forecasts 167
- Using lookup to get your items forecast 170
- Exporting and visualizing your forecasts 172
    - Exporting the predictions 172
    - Visualizing your forecast's results 174
    - Performing error analysis 176
- Generating explainability for your forecasts
    - Generating forecast explainability 180
    - Visualizing forecast explainability 182
- Summary 183

### 7 Improving and Scaling Your Forecast Strategy

- Technical requirements 186
- Deep diving into forecasting model metrics 186
    - Weighted absolute percentage error (WAPE) 187
    - Mean absolute percentage error (MAPE) 189
    - Mean absolute scaled error (MASE) 190
    - Root mean square error (RMSE) 190
    - Weighted quantile loss (wQL) 191
- Understanding your model accuracy 192
- Model monitoring and drift detection 196
- Serverless architecture orchestration 196
    - Solutions overview 197
    - Solutions deployment 201
    - Configuring the solution 206
    - Using the solution 207
    - Cleanup 214
- Summary 215

## Section 2: Detecting Abnormal Behavior in Multivariate Time Series with Amazon Lookout for Equipment

### 8 An Overview of Amazon Lookout for Equipment

- Technical requirements 220
- What is Amazon Lookout for Equipment? 220
- What are the different approaches to tackle anomaly detection? 221
    - What is an anomaly? 222
    - Model-based approaches 223
    - Other anomaly detection methods 225
- Using univariate methods with a multivariate dataset 225
- The challenges encountered with multivariate time series data 226
- How does Amazon Lookout for Equipment work? 227
    - Defining the key concepts 227

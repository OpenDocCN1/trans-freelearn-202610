## 索引

### A

access token
  用于配置 Azure Databricks CLI 59, 60
all-purpose clusters 9
Apache Kafka 174-176
Apache Spark 4, 5
application programming interfaces (APIs) 230
area under the curve (AUC) 267
Atomicity, Consistency, Isolation, and Durability (ACID) 107
authentication 34
authorization 34
Auto Compaction 144
Auto Loader 173, 174
AutoML
  关于 303
  超参数调优 302
Avro data 176-178
AWS services 6
Azure AD token
  用于配置 Azure Databricks CLI 60
Azure Blob storage
  关于 79
  访问密钥，上传 81, 82
  连接，设置到 83
  文件，上传 81, 82
  设置 80, 81
  与 Azure Databricks 配合使用 79
Azure Cloud Shell
  Databricks CLI 64, 65
Azure Databricks
  关于 5, 6
  从中访问 ADLS Gen2 76, 77
  架构，检查 6, 7
  使用 Azure Blob storage 79
  使用 Bloom filters 153
  集群库 200-204
  Clusters CLI 63
  处理连续数据流 168
  使用 Horovod 分布式学习库 361-364
  Jobs CLI 63
  使用作业编排作业 91
  安装库 200
  笔记本作用域库 200-205
  与 S3 配合使用 78
  用于调度作业 101
  与 VNets 配合使用 53, 54




Workspace CLI 61
workspace libraries 200-203
Azure Databricks authentication
  access control 52, 53
  examining 51
Azure Databricks CLI
  configuring, with access token 59, 60
  configuring, with Azure AD token 60
  installation, validating 61
  setting up 58, 59
  using, to explore workspace 62
Azure Databricks cluster
  configuring 44, 45
Azure Databricks, core concepts and terminology
Apache Spark Context/Environment 10
apps 10
clusters 9
dashboards 10
data 8
discovering 8
experiments 11
jobs 10
libraries 11
models 11
notebooks 8
tables 11
workspace filesystem 11
workspaces 8
Azure Databricks File System (ADFS) 77
Azure Databricks notebooks
  attaching, to clusters 19, 20
  cell result, downloading 24
  cells 21
  cells, running 22
  command comments 24
  connecting, to clusters 18
core functionalities 21
creating 16, 17
default language 22, 23
documentation 23
execution contexts 18
exporting 18
external notebook formats 17
idle execution contexts 19
importing 18
managing 16, 17
scheduling 21
SQL code, formatting 25
toolbar 21
using 16
Azure Databricks workspace
  about 12
  assets 12, 13
  creating, with Azure Resource Manager templates 56-58
  interacting with 12
  object operations 15, 16
Azure Databricks workspace, assets
  about 12, 13
  folders 13
  user home folders 14, 15
Azure Databricks workspace, folders
  root folder 14
  special folders 14
Azure Data Factory (ADF)
  about 92
  extract, transform, and load (ETL), creating 94-101
  resource, creating 92, 93
Azure Data Lake
  back table data, writing to 89-91
Azure Data Lake Storage (ADLS) 107

## C

## B

## D

- Azure Data Lake Storage
  - Gen2 (ADLS Gen2)
    - accessing, from Azure Databricks 76, 77
    - data lake, setting up 70, 72
    - data, loading from 77, 78
    - data, uploading 73-75
    - using 70
- Azure Event Hubs 171-173
- Azure portal UI
  - used, for setting up Databricks workspace 40-42
- Azure Resource Manager templates
  - about 56
  - deployed resources, reviewing 58
  - resources, cleaning 58
- Azure Resource Manager
  - templates, modes
  - complete mode 56
  - incremental mode 56
- bag of words (BOW) 257
- binarization 237, 238
- Blob Storage 6
- Bloom filter index
  - creating 154
- Bloom filters
  - using 151-153
  - using, in Azure Databricks 153
- Bokeh
  - using, for data visualization 223, 224
- Bucketizer class 246-249
- central processing unit (CPU) 365
- cloudFiles 110
- cluster access control 34
- cluster libraries
  - about 200
  - installing 203, 204
- cluster permissions
  - configuring 34, 35
- Clusters CLI 63
- comma-separated values (CSV) 230
- computation management
  - cluster information 32
  - cluster logs 33
  - clusters, beginning 30, 31
  - clusters, deleting 32
  - clusters, displaying 30
  - clusters, terminating 31
  - exploring 29
- control plane 7
- convolutional neural network (CNN) 367
- CSV files
  - DROPMALFORMED mode 233
  - FAILFAST mode 233
  - PERMISSIVE mode 233
  - reading 232, 233
- Current Population Survey (CPS) 75
- data
  - loading 230
  - loading, for deep learning 274
  - loading, from ADLS Gen2 77, 78
  - loading, with TFRecordDataset 282-284
  - managing, with TFRecords 277-279

partitioning, to speed up
  queries 114, 115
reading, from CSV files 232-234
reading, from DBFS 230, 231
uploading, to ADLS Gen2 73-75
Databricks 5
Databricks CLI
  from Azure Cloud Shell 64, 65
Databricks File System (DBFS)
  data, reading 230-232
Databricks workspace
  setting up, with Azure portal UI 40-42
data definition language (DDL) 83
DataFrame, of images
  featurization, applying to 298, 299
data ingestion, with Delta Lake
  about 108
  Auto Loader 110, 111
  COPY INTO SQL command 109
  partner integrations 108
data management
databases and tables 25
databases and tables, viewing 26
data, importing 27, 28
exploring 25
table, creating 28
table details 29
data manipulation language (DML) 83
data plane 7
data preprocessing 293
dataset
generating 285, 286
reading, with Petastorm 287, 288
data sinks
integrating 178-181
data visualization
about 222
Bokeh, using 223, 224
Matplotlib, using 225, 226
Plotly, using 227, 228
deep learning
data, loading for 274
distributed training 358-360
Petastorm, used for data
  preparation 289-291, 293
deep neural networks (DNNs) 358
Delta Engine
  used, for optimizing file
    management 134
Delta Lake
  about 106-108
  initial position, specifying 122, 123
  modes, streaming 123, 124
  optimization with 124-131
  using, for data ingestion 108
Delta Lake, features
  ACID transactions 107
  schema variations, handling 107
  streaming and batch unification 107
  time travel 107
  upserts and deletes 108
Delta Lake tables
  creating 112, 113
Delta tables
  reading 113
  using, as stream source 168-171
  streaming from 121
distributed learning
  Petastorm, using 284
  TFRecords, using 275
distributed training
  for deep learning 358-360
  with HorovodRunner 369-373
distributing hyperparameter tuning
  Horovod, using 373-375
  Hyperopt, using 373-375

## E

## F

## G

## H

dummy variables 241
dynamic file pruning (DFP)
  used, for optimizing queries 149
  using 150, 151

element-wise product 249
eviction 19
experiment 305
external notebook formats 17
extract, transform, and load (ETL)
  creating, in Azure Data Factory (ADF) 94-101

feature engineering
  about 235, 236
  goals 234
  strategies 235
feature hashing 257
featurization
  about 293
  applying, to DataFrame of images 298, 299
  with pandas UDFs 295-298
  with pre-trained model, for transfer learning 294
file detection modes, Auto Loader
  directory listing 173
  file notification 173
file management optimization, with Delta Engine
  about 134
Apache Spark caching 145-147
Auto Optimize feature 143, 144
caching, using 145

checkpoints 140, 141, 142
data recency, managing 140
data, skipping 136, 137
data subset, caching 147, 148
Delta cache, configuring 149
Delta caching 145
  small files, merging with bin-packing 134-136
ZORDER clustering, using 137-139
Filesystem in Userspace (FUSE) 230
folder permissions
  about 35
  configuring 36
  default permissions 35, 36

gigabytes (GB) 231
graphics processing unit (GPU) 361
Groups API 64

Horovod distributed learning library
  installing 365
  model, training on single node 366-369
  using 365, 366
  using, in AzureDatabricks 361-364
Horovod 313
Hyperopt
  best practices, applying 320
  concepts 314-318
hyperparameter tuning 313
  model selection, optimizing 321-329
  search space, defining 318
hyperparameters, deep learning
  examples 302

hyperparameter tuning
  with AutoML 302
  with Hyperopt 313

I

incrementalization 163
invalid actions 141

J

Java Database Connectivity (JDBC) 165
JavaScript Object Notation (JSON) 353
job clusters 9
jobs
  orchestrating, with Azure Databricks 91
  scheduling, with Azure Databricks 101
Jobs CLI
  about 63
  used, for listing and finding jobs
    in Azure Databricks 63
jobs, scheduling with Azure Databricks
job logs 104
  notebook, scheduling as job 102, 103
join performance
  optimizing 155

K

k-nearest neighbors (KNN) 244
Koalas API
  using 254, 255

M

machine learning (ML) 70
machine learning models
  managing 332
machine learning models,
  training on tabular data
  about 261
MLflow model serving 269, 271
ML model, building 263- 267
registering, in MLflow Model
  Registry 268, 269
variables, engineering 262, 263
Matplotlib
  using, for data visualization 225, 226
mean-squared error (MSE) 340
missing values
  handling 255, 256
MLEap
  pipelines, exporting 345-51
  pipelines, loading 345-51
MLflow
  about 303, 304
  components 304
  model selection, optimizing 321-329
  models, serving 351, 352
  model tracking, automating 304
MLflow API
  used, for registering model 336
MLflow, core concepts
  Activities 334
  Description 334
  Model 333
  Model Stage 333
  Model Version 333
  Registered Model 333
MLflow, information
  artifacts 305
  metrics 305
  parameters 305
  source 305
  start and end time 305



tags 305
version 305
MLflow model permissions
  about 37
  configuring 37
  default permissions 37
MLflow models 304
MLflow notebook experiments
  using 334
MLflow projects 304
MLflow runs
  managing 306
MLflow tracking
  automating, with MLlib 307-312
MLflow Tracking 304, 305
MLlib Pipelines, components
  DataFrame 307
  Estimator 308
  Parameter 308
  Pipeline 308
  Transformer 307
Model Registry
  about 304
  example 339-344
models
  registering, with MLflow API 336
  scoring 353-355
  serving, with MLflow 351, 352
model selection
  optimizing, with Hyperopt 321-329
  optimizing, with MLflow 321-329
  optimizing, with scikit-learn 321-329
model stage
  transitioning 337-339
model tracking
  automating, with MLflow 304
Modified National Institute of Standards and Technology (MNIST) 365

## N
nested MLflow runs
  child runs 306
  main run 306
network security group (NSG) 54
n-grams 284
Normalizer class 244
Not a Number (NaN) 248
notebook
  creating 46-51
notebook experiment 335
notebook permissions
  about 36
  configuring 36
notebook-scoped Python libraries
  about 200
  installing 205-209
  installing, methods 205

## O
offset 174
one-hot encoding 241, 242
Optimized Writes 144
Out of Memory (OOM) errors 298
output modes
  append mode 163
  complete mode 163
  update mode 163

## P
pandas Dataframe API (Koalas)
  about 216
  SQL, using 221
  using 216-219, 221
pandas UDFs
  about 294
  used, for performing
    featurization 295-298
Parzen estimators 315
Petastorm
  about 284
  characteristics 274
  used, for preparing data for
    deep learning 289-293
  used, for reading dataset 287, 288
  using, for distributed learning 284
pipelines
  exporting, with MLEap 345-351
  loading, with MLEap 345-351
Plotly
  using, for data visualization 227, 228
polynomial expansion 238, 239
pre-trained model, for transfer learning
  used, for performing featurization 294
process pool 285
pyll graphs 319
PySpark
  MLlib 210
  PySpark DataFrames 209
  PySpark SQL module 209
  Resilient Distributed Datasets (RDDs) 210
  streaming source 209
  working with 221, 222
PySpark application programming interface (API)
  about 84, 209
  functionalities 209, 210
PySpark DataFrames
  operating with 210-215
PySpark MLlib 313
Python 6

## Q
queries optimization
  DFP, using 149
query failures
  recovering from 181, 182
querying, with Structured Query Language (SQL)
  about 84
  tables, creating in data lake 85-89
  views, creating 84, 85

## R
range join optimization
  about 155
  enabling 156, 157
read-eval-print-loop (REPL) 18
receiver operating characteristic (ROC) 264
relationships and columns
  specifying, with skew command 158, 159
retention period 174
ring allreduce technique 360, 361
R-squared (R2) metric 335
rules for reconciliation 141
runs 305

## S
S3 buckets 6
Scala 6
schema
  validating 120
schema inference
  automating 280-282
scikit-learn
  model selection, optimizing 321-329
search space
  defining 318
shared access signature (SAS) 172
Simple Storage Service (S3)
  about 163
  connecting to 78
  using, with Azure Databricks 78
skew join optimization 157
Spark data frames
  about 78-84
  data, cleaning 83
  data, loading into 78, 79
  data, transforming 83
SparkSession 10
Spark TensorFlow Distributor package
  using 376-381
SQL databases 6
StandardScaler class 245, 246
streaming data frames
  data, visualizing 185-187
streaming queries
  optimizing 183
streaming query executions
  triggering 183
StringIndexer class 240, 241
Structured Query Language (SQL) 83, 232
Structured Streaming
  example 188-196
Structured Streaming API
  aggregations, filtering 166
  aggregations, mapping 166
  aggregations, running 166
  interactive queries 167
  streaming, merging with static data 167
  using 164, 165
windowed aggregations, performing
  on event time 166
Structured Streaming model 163, 164

## T
table
  past data, working with 118-121
  past states, querying 115
  present data, working with 118, 119
  querying, with time travel 115-118
table read and writes
  batching 112
  streaming 121, 122
table updates and deletes
  managing 122
Term Frequency-Inverse Document
  Frequency (TF-IDF) 257
  features, extracting 258, 259
text
  features, extracting 257
TFRecord
  characteristics 274
  files, structuring 275-277
  used, for managing data 277-279
  using, for distributed learning 275
thread pool 285
time-series data
  joining 252, 253
time-series data sources 250-252
tokenization 236, 237
Tree of Parzen Estimators
  (TPE) algorithm 327
triggers
  based on fixed-interval
    micro-batches 184

## U
Uniform Resource Identifier (URI) 339
user-defined function (UDF) 269
user interface (UI) 92

## V
VectorIndexer 243, 244
VNet Injection 53
VNets
  deploying 54
  deployment requirements 54
  virtual network, configuring 55
  virtual network requirements 54
  working with, in Azure
    Databricks 53, 54

## W
Word2vec 259, 261
word embeddings 259
Workspace CLI 61
workspace experiment
  about 335
  creating 335
workspace libraries
  about 200
  installing 201, 202
  removing 203
Workspace UI, Azure CLI
  accessing 43, 44
continuous trigger, with fix
  checkpoint interval 184
examples 184, 185
one-time micro-batch triggers 184
types 184
unspecified trigger 184

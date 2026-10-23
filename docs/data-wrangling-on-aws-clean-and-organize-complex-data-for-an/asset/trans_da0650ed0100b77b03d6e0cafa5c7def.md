

or more Amazon Redshift clusters in Data Wrangler. Additionally, Data Wrangler offers seamless integration with Snowflake, allowing you to effortlessly prepare data stored in Snowflake for ML tasks. With a simple and intuitive interface, you can establish a direct connection to Snowflake without the need for any coding. Moreover, Data Wrangler facilitates data integration by enabling you to combine your Snowflake data with information residing in Amazon S3, as well as data extracted through Amazon Athena and Amazon Redshift. If you prefer to utilize Databricks as your data source, Data Wrangler supports connection to this data processing service as well. By leveraging the **Java Database Connectivity (JDBC)** import functionality, you can import data assets from your Databricks database. Once connected, you can specify a SQL query to extract the required data and seamlessly import it into Data Wrangler for further processing.

## Data orchestration

Data orchestration can be defined as the process of combining data from various sources, including the steps to import, transform, and load it to the destination data source, with the fundamental principle being the ability to automate all the steps involved in the data preparation steps in a repeatable and reusable form, which can then be integrated with the overall ML pipelines. While data orchestration can be used in a wider context that can also include resource provisioning, scaling, and monitoring, the core of data orchestration is creating and automation data workflows, and this is where we will focus for the remainder of the book. The other heavy-lifting tasks of provisioning, scaling, and monitoring are taken care of by AWS.

SageMaker Data Wrangler uses a data flow to connect the datasets and perform transformation and analysis steps. This data flow can be used to define your data pipeline and consists of all the steps that are involved in data preparation. When you create a data flow in Data Wrangler, it automatically provisions the resources needed to run your analysis and transformations that you will define inside your flow. While there is a default option, you also have the flexibility of choosing a supported instance type based on your specific data preparation needs.

Data flow creation in SageMaker Data Wrangler is GUI based. When you first import your data (or use a sample dataset), Data Wrangler automatically infers the data types of the columns in your data source and creates a new data frame. Both the data source and the data frame appear in the data flow UI of Data Wrangler. Data Wrangler allows you to easily add additional steps to your data pipeline using the data flow UI.

Data Wrangler steps can be categorized as transform, analysis, join, and concatenate. A transform step—as the name indicates—consists of applying transformations to your data. A join step includes a join operation with another dataset. A concatenate step, on the other hand, includes a concatenate operation with another dataset. You can use the analysis step to add ad hoc analysis that may be helpful with your data preparation.

Spending time on data preparation early in the ML life cycle can significantly improve the model performance and also significantly reduce training time and cost. In most real-life scenarios, data preparation is iterative and includes multiple passes; as such, you will end up doing multiple transformations on the same dataset, and these will appear stacked in the data flow UI. All the steps in your data flow can be updated or deleted directly from the data flow UI (shown in *Figure 4.2*) or the table UI:

![](img/e28602b44ec813a47df57c96b204412c_154_0.png)

Figure 4.2 – Data Wrangler data flow UI

We will go through the details of a data flow in the next chapter, where we will cover a step-by-step example of creating a data flow. All the steps in your data flow can be saved and exported as a file with the .flow extension. This enables the process to be repeatable, version controlled, and integrated with the overall ML pipeline. Additionally, you can also use AWS services such as Amazon EventBridge and AWS Lambda to schedule automatic runs of these data flows. Finally, since this orchestration is logically separated from compute and storage, you can optimize resource utilization by turning off instances when not in use and still retain all the steps in the flow that can be executed as needed.

## Data transformation

Data processing for ML primarily includes data transformation. At its core, SageMaker Data Wrangler includes over 300 built-in transformations that are commonly used for cleaning, transforming, and featuring your data specifically for data science and ML. Using these built-in transformations, you can transform columns within your dataset without having to write any code. In addition to these built-in transformations, you can add custom transformations using PySpark, Python, pandas, and PySpark SQL.

Some of these transformations operate in place, while others create a new output column in your dataset. Whenever you incorporate a transform into your data flow, it introduces a new step in the process. Each added transform modifies your dataset and generates a fresh data frame as a result. Subsequently, any subsequent transforms you apply will be performed on this updated data frame.

In the real world, datasets are often imbalanced. This imbalance can be in the form of an underrepresented class or sometimes even a complete absence due to sampling errors. Needless to say, this is one of the starting issues that must be tackled in data processing and can also be time-consuming to detect and fix. SageMaker Data Wrangler includes a balance data operation that enables data practitioners to quickly balance imported datasets. The balance operation within Data Wrangler enables standard data balance operations. These include the following:

- **Random oversampling**: This operation randomly duplicates instances from the minority class
- **Random undersampling**: Similar to random oversampling, this operation randomly removes samples from the overrepresented class
- **Synthetic Minority Oversampling Technique (SMOTE)**: SMOTE utilizes samples from the underrepresented category to interpolate new synthetic minority samples

In addition to the built-in one-click transforms, Data Wrangler also supports custom transforms. Custom transforms enable data scientists to work with packages and frameworks that they are already familiar with to write custom transform operations. These include the use of defined functions via Python, PySpark, PySpark (SQL), or pandas. Data Wrangler also allows you to use popular libraries by importing them in the custom code block of the custom transform operation. There are, however, some restrictions using custom transform operations, which we will cover in depth in the next chapter.

You can also implement custom formulas via PySpark SQL to generate new columns or features for your dataset. As we briefly looked at in the *Data orchestration* section, SageMaker Data Wrangler data transformations also support standard join and concatenation operations.

SageMaker Data Wrangler also works with non-numeric data. It includes built-in transforms for one-hot encoding, categorical encoding, and similarity encoding, among others. In addition, depending on the type of data science or ML problem at hand, it might be useful to featurize text. Data Wrangler transforms include capabilities to vectorize text data—that is, create meaningful numeric embeddings from text data in the form of vectors, which can then be used to perform numeric operations.

Another commonly seen data type in ML is time series data. This is data that is indexed to a specific time. SageMaker Data Wrangler transforms include a number of options to work with time series data. This includes commonly used operations such as grouping time series data, handling missing values within time series data, extracting features from time series data, and using rolling windows within time series data.

The following screenshot shows the built-in transformations available in Data Wrangler:

![](img/e28602b44ec813a47df57c96b204412c_155_0.png)


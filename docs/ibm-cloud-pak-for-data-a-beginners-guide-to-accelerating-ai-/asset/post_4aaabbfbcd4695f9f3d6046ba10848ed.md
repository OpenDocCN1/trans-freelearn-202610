
The following diagram showcases the ModelOps component:

![](img/50e1dac0750b69f2b8e8efff000eb13b_187_0.png)

*Figure 8.1 – ModelOps component diagram*

The featured **Cloud Pak for Data** services include Watson Knowledge Catalog, Watson Studio, and Watson Machine Learning:

- **Data steward(s)** organize data in the catalog and create governance policies and rules. Additionally, they create data rules that obfuscate parts of the datasets to ensure only authorized personnel have access to data.
- **Data scientist(s)** leverage the wide variety of tools/technologies that are available (Jupyter notebooks, **SPSS modeler**, **RStudio**, and so on) to prepare data and train AI models.

The outcome is that, based on member risk level (high, medium, or low risk), this customer was able to prioritize outreach activities. Cloud Pak for Data helped save lives by helping to proactively prioritize the relevant patients based on their risk exposure.

Next, we will cover another customer use case related to voice-enabled chatbots.

## Voice-enabled chatbots

The customer challenge included a public petroleum and natural gas company that was struggling to locate consistent and complete information on the services provided by the Information Technology (IT) and Engineering departments. Their existing system was a text-based transactional chatbot lacking governance and adoption due to the quality of responses.

With Cloud Pak for Data, they were able to restructure the text-based transactional chatbot with a voice-enabled, avatar-styled interface for improved user experience and, more importantly, better responses to user queries. This, combined with structured analytics, helped to drastically improve customer satisfaction. They used the SPSS modeler and Jupyter notebooks from the Watson Studio service for structured analytics, Watson Discovery for textual search, the Watson API service for voice transcription, and Watson Assistant for an avatar-styled chatbot experience.

The following diagram represents the AI-powered chatbot component:

![](img/50e1dac0750b69f2b8e8efff000eb13b_188_0.png)

Figure 8.2 – AI-powered chatbot component diagram

The featured **Cloud Pak for Data** services include Watson Knowledge Catalog, Watson Discovery, Watson Assistant, Watson API, and Watson Studio:

- **Data steward(s)** organize data in the catalog and create governance policies and rules. Additionally, they create data rules that obfuscate parts of the datasets to ensure only authorized personnel have access to data.
- **Business analyst(s)** analyze the business needs and define application requirements, including channel-specific interactions.
- **Data scientist(s)** design and train AI models to derive context from existing content and desired dialog(s) for the assistant.
- **Application developer(s)** use trained models to integrate these AI models into applications for the different channels.

The outcome of the use case was that, with this Cloud Pak for Data powered solution, this customer now has an improved service accessibility time. They also helped their employees get the information they needed in real time. Their future plans include delivering multiple AI-powered chatbots while expanding on the overall governance strategy.

Let's move on to the next use case, which is related to risk and control automation.

## Risk and control automation

The customer challenge is that a major financial institution requires its audit departments to track and evaluate controls that help them determine their level of risk exposure. Considering that any major bank has 250,000+ controls that are mapped to risks, this was not an easy task.

A good business control clearly states who, what, when, how, and where the control is to be used. Banks often suffer due to poorly defined controls. With Cloud Pak for Data, this customer was able to train an ML model to predict the *Control* quality level of each written control. They used the Watson Studio service to train the model and Watson Machine Learning to deploy and operationalize the model.

## Enhanced border security

The customer challenge was that a sophisticated customs department, managing how goods were imported and exported into the country, wanted to leverage AI models to identify risky goods entering the country, while also reducing the number of false positives in identifying these risky goods. The client was using a rules-based engine to identify risky goods. However, the vast majority were false positives, which resulted in needless inspections and slowed down the entire customs process.

With Cloud Pak for Data, this customer was able to build a unified platform to handle all of their data science needs – from discovery to deployment. They used platform connectivity to gain access to existing sources of data, the Watson Studio service to train ML/AI models, the Watson Machine Learning service to deploy and operationalize the models, and Watson OpenScale to monitor these models and understand the logic behind them.

The following diagram represents an enhanced border security component:

![](img/50e1dac0750b69f2b8e8efff000eb13b_191_0.png)

Figure 8.4 – Enhanced border security component diagram

![](img/50e1dac0750b69f2b8e8efff000eb13b_193_0.png)

*Figure 8.5 – Data Fabric component diagram*

The featured **Cloud Pak for Data** services include Db2 Warehouse, MongoDB, data virtualization, Watson Knowledge Catalog, DataStage, and Cognos Analytics:

- **Data engineer**(s) provision data management capabilities and populate data – they use a MongoDB instance for operational data, a Db2 Warehouse instance for storing analytic data, and a data virtualization instance to virtualize data residing in the CP4D platform and other sources of data across the enterprise.
- **Data engineer**(s) provision **Extract, Transform, and Load (ETL)** capabilities with DataStage. This allows data to be sourced from a variety of sources, transforming it with a choice of operators and then loading it to the target repositories.
- **Data steward(s)** organize data in the catalog and create governance policies and rules. Additionally, they create data rules that obfuscate parts of the datasets to ensure only authorized personnel have access to data.
- **Data analyst(s)** and **Business analyst(s)** create reports and dashboards using the easy-to-use drag and drop Cognos Dashboard interface. For more sophisticated reports/scorecards/dashboards, they must work with the **Business Intelligence (BI)** team and leverage the Cognos Analytics BI service.

The outcome of this use case is that the client was able to realize the value of Unified Data Fabric for improving the ease of data access through a governed semantic layer on Cloud Pak for Data. Their ability to easily identify and select cohorts and gain deeper insights were greatly improved, and the platform allowed for more self-service analytics.

Next, we have a customer use case on financial planning and analytics.

## Financial planning and analytics

The customer challenge was that an online distribution company wanted to scale its planning operations and bring cross-organizational collaboration into the planning process. They had challenges in scaling their existing planning solution, which also lacked effective collaboration capabilities.

Traditionally, all businesses do **Financial planning and analytics (FP&A)**. This involves multiple finance teams using hundreds of interconnected spreadsheets for planning and reporting. This is a very time-consuming, error-prone process and limits the amount and number of scenarios that can be used. In this era, organizations are looking to go beyond finance to a more collaborative and integrated planning approach known as FP&A. This involves multiple teams throughout the organization all collaborating with the goal of gaining forecast accuracy and the ability to quickly respond to changes in market conditions.

The planning analytic service in CP4D helps us move past this manual planning, budgeting, and forecasting process to an automated AI-based process. You can identify meaningful patterns and build what-if scenarios to create models. This, combined with the integrated Cognos Analytics service, enables self-service analytics with stunning visualizations and reports. CP4D, with its data management capabilities and centralized governance catalog, provides access to trusted data for analytics.

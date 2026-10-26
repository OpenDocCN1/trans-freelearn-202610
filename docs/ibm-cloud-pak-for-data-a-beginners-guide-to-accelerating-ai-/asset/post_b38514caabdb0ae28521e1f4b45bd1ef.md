

**Cloud Pak for Data**, with its centralized governance catalog and dashboarding service, enables self-service analytics and the confident sourcing of data from governed data assets.

## BI and reporting

BI is a capability that ingests business data and presents it in user-friendly views such as reports, dashboards, charts, and graphs. BI tools enable business users to access different types of data, including historical and current, third-party, and in-house, as well as semi-structured data and unstructured data such as social media. Users can analyze this information to gain insights into how the business is performing. Although BI does not tell business users what to do or what will happen if they take a certain course of action, neither is BI only about generating reports. Instead, BI offers a way for people to examine data to understand trends and derive insights.

## Predictive versus prescriptive analytics

Predictive analytics is a practice of using advanced algorithms and ML to process historical data, learning what has happened while uncovering unseen data patterns, interactions, and relationships. Predictive models provide actionable insights, but they don't say what action needs to be taken based on insights for best outcomes.

Prescriptive analytics, on the other hand, enable accurate decision-making for complex problems by using optimization models that are mathematical representations of business problems. These optimization models use solvers with sophisticated algorithms.

Often, we might question whether we should use predictive analytics or prescriptive analytics. The answer to that is we need both, and to illustrate the point, let's use a simple example. Throughout the Covid-19 pandemic, ML models have been used to forecast the demand for protective masks, but complementing this information with prescriptive analytics will help build replenishment plans to decide which distribution center should supply to different locations to adequately meet demand.

Let's start by demystifying these three concepts, as follows:

- **AI**—AI systems can be thought of as expert systems that can connect different data points and make decisions based on complex rules that mimic human behavior, free of emotions. AI is used to predict, automate, and optimize tasks that have historically been done by humans, such as speech recognition, facial recognition, translation, and so on.
- **ML**—A subset of AI applications that learns by itself, using new data points to predict outcomes with increasing accuracy.
- **DL**—A subset of ML where neural networks learn from large volumes of data. DL algorithms perform a task repeatedly and gradually improve the outcome through deep layers that enable progressive learning.

The following diagram shows the relationship between AI, ML, and DL:

![](img/50e1dac0750b69f2b8e8efff000eb13b_111_0.png)

Figure 5.2 – Types of AI

With a better understanding of AI, ML, and DL, we will now look at the existing methodology for ML projects and additional considerations for new-age AI applications to transform insights into action.

## AI life cycle – Transforming insights into action

Enterprises going through digital transformation infuse AI into their applications; a well-defined and robust methodology is required to manage the AI pipeline. Traditionally, a **cross-industry standard process for data mining (CRISP-DM)** methodology was used for ML projects, and it is important to understand this methodology before we explore the challenges faced with the implementation of an AI-driven application and how a more comprehensive AI life cycle will help with this process.

CRISP-DM is an open standard process model that describes the approach and the steps involved in executing data mining projects. It can be broken down into six major phases, as follows:

- **Business understanding**
- **Data understanding**
- **Data preparation**
- **Model building**
- **Model evaluation**
- **Model deployment**

The sequence of these phases is not strict and is often an iterative process. The arrows in the following diagram indicate the most important and frequent dependencies between the different phases, while the outer circle symbolizes the cyclic nature of data mining:

![](img/50e1dac0750b69f2b8e8efff000eb13b_112_0.png)

Figure 5.3 – Analytics life cycle

This worked great in the past, but with the increasing amount of data being collected, we now have to consider the volume, variety, and velocity of data and how it affects the AI life cycle. What this means is we need to place more emphasis on the data collection, data governance, and continued model evaluation phases. Model evaluation is no longer just about evaluating the quality of the model—there are also other considerations, including the fairness and explainability of the models being deployed.

Here are some key considerations when enabling this AI life cycle:

- Lowering the skillset to build and operationalize AI
- Accelerating delivery time by minimizing mundane tasks
- Increased readiness for AI-infused applications through optimizing model accuracy
- Delivering real-time governance of AI models, improving trust and transparency

With a good understanding of the additional considerations for AI applications, we will next delve into the need for AI governance as a foundation of trust and transparency in new-age AI applications.

## AI governance: Trust and transparency

AI is prevalent in every industry—everything from recommendation engines to tracking the spread of the COVID-19 virus. This application of AI empowers organizations to innovate and introduce efficiency, but it also introduces risks around how AI is employed to make business decisions.

In general, governance usually relates to who is responsible for data, how data is handled, who has access to see particular datasets in an enterprise, and how they get to use the data. This same notion of establishing a governance framework to protect the data and the consumers also applies to AI models, which may exhibit behavior that may be unfair or biased—or both—to some individuals or groups of people. Data governance has evolved over a period, and there are well-established methodologies and best practices, but we now need to do something similar for AI models. The challenge with traditional data governance is the increasing volume of data and the need to identify and align data/technical assets to business assets that are organized into subjects and logical areas and establishing policies and rules that dictate the use of data assets. However, with AI models, there are several considerations, including the following ones:

- Multiple versions of the model
- Different input datasets used to train the model
- Pipeline information on any/all transformations to the model input attributes
- Algorithm/technique used to train the model
- Hyperparameter optimization
- Training/test datasets used to build and test the model

Explainable AI is not just about being able to interpret the model output—it is also about being able to explain how the model was built, which input data selection process and filters were used, whether the selected dataset was in compliance with organizational policies and rules, and if there was any intentional or unintended bias in the selected datasets.

## Automating the AI life cycle using Cloud Pak for Data

After an organization has been able to collect its data and organize it using a trusted governance catalog, it can now tap into the data to build and scale AI models across the business. To build AI models from the ground up and scale it across the business, organizations need capabilities covering the full AI life cycle, and this includes the following:

- **Build**—This is where companies build their AI models.
- **Run**—After a model has been built, it needs to be put into production within an application or a business process.
- **Manage**—After a model is built and running, the question becomes: *How can it be scaled with trust and transparency?* To address complex build and run environments, enterprises need a tool that not only manages the environment but also explains how their models arrived at their predictions.

Let's exercise **Model Operations** (**ModelOps**) using Cloud Pak for Data to understand how the platform can be used to automate the AI life cycle. ModelOps is a methodology and process of building data science assets that can quickly progress from development to production.


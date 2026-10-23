
# 11

# AI Center
Advanced Topics

In the previous chapters, we saw how we could leverage **machine learning (ML)** to assist with receipt processing and email classification. While these use cases are very popular, there are many other types of use cases that **Named Entity Recognition (NER)** models or custom ML models can be applied to.

In this chapter, we will cover the following topics:

- **Named Entity Recognition (NER)** with AI Center
- Deploying your own custom models to AI Center

We will introduce the concept of NER, a task that opens new types of use cases we can apply ML to. We will also venture into deploying custom Python ML models into AI Center, as there are many instances where organizations have their own prebuilt ML models that can be leveraged with RPA.

Once complete, we will have an understanding of how to leverage UiPath's NER capabilities, as well as how to connect your own custom Python models to AI Center.

## Technical requirements

All code examples for this chapter can be found on GitHub at `https://github.com/PacktPublishing/Democratizing-Artificial-Intelligence-with-UiPath/tree/main/Chapter11`.

Working with UiPath is very easy, and with the Community version, we can get started for free. However, for UiPath AI Center, we will need an enterprise license. You can acquire a 60-day enterprise trial license from `uipath.com`.

With the 60-day enterprise trial, we will have access to the following:

- 5 RPA Developer Pro licenses – Named User licenses include access to Studio, StudioX, Attended Robot, Apps, Action Center, and Task Capture
- 5 Unattended Robots, 5 Testing Robots, 2 AI Robots
- AI Center, AI Computer Vision, Automation Hub, Data Service, Document Understanding, and Insights

For this chapter, you will require the following:

- UiPath Enterprise Cloud (with AI Center)
- UiPath Studio 2021.4+
- `UiPath.MLServices.Activities` package v.1.20.0 or higher

> **Important Note**
> 
> Directions on how to install UiPath packages can be found at `https://docs.uipath.com/studio/docs/managing-activities-packages`.

## Enabling AI Center in UiPath Enterprise trial

Additional technical requirements include leveraging AI Center. UiPath Document Understanding comes out of the box with the UiPath Community and Enterprise versions, however, with UiPath AI Center, we need to enable the service within the Enterprise trial. You can enable the service by following the given steps:

1. Navigate to **Automation Cloud**: `https://cloud.uipath.com`.

![](img/05268897c8ac5d80eb3fead1cc8a822a_347_0.png)

Figure 11.1 – UiPath Automation Cloud home page

2. Navigate to **Admin**.

![](img/05268897c8ac5d80eb3fead1cc8a822a_347_1.png)

Figure 11.2 – Click on Admin

3. In **DefaultTenant**, click on the three dots and then click on **Tenant Settings**.

![](img/05268897c8ac5d80eb3fead1cc8a822a_348_0.png)

Figure 11.3 – Click on Tenant Settings

4. Choose **AI Center** under **Provision Services**, and click on **Save**.

![](img/05268897c8ac5d80eb3fead1cc8a822a_348_1.png)

Figure 11.4 – Check AI Center, then click Save

Once AI Center is enabled, we have completed the technical requirements for this chapter. In the next section, we will start gathering context about the use case by reviewing the current state.

These types of use cases cover a lot of automation opportunities, including the email classification use case from an earlier chapter.

To build these use cases, UiPath's AI Center comes with both open source NER models and a UiPath-prepared NER model (*Figure 11.6*).

![](img/05268897c8ac5d80eb3fead1cc8a822a_350_0.png)

Figure 11.6 – AI Center NER models

Within **Open-Source Packages > Language Analysis**, you will find an ML package named NamedEntityRecognition. This ML package is pre-trained to recognize 18 types of different named entities, listed in *Table 11.1*.

| Entity | Description |
|---|---|
| PERSON | People, including fictional |
| NORP | Nationalities, religious, political groups |
| FAC | Buildings, airports, highways, bridges |
| ORG | Companies, agencies, institutions |
| GPE | Countries, cities, states |
| LOC | Non-GPE locations, mountain ranges, bodies of water |
| PRODUCT | Objects, vehicles, foods |
| EVENT | Named hurricanes, battles, wars, sports events |
| WORK_OF_ART | Titles of books, songs |
| LAW | Named documents made into laws |
| LANGUAGE | Any named language |
| DATE | Absolute or relative dates or periods |
| QUANTITY | Measurements, as of weight or distance |
| ORDINAL | "first," "second" |
| CARDINAL | Numerals that do not fall under another type |

Table 11.1 – Entities of the NamedEntityRecognition model

As shown, the NamedEntityRecognition open source model can be used to identify many different entities. The one caveat of this model is that it comes pretrained and is not re-trainable. In the next section, we will look into UiPath's custom NER ML model for use cases that require a trainable NER model.

## Custom NER with UiPath

For cases when we require an NER model that does not fit into the 18 entities of the open source NER model, or for cases that require retraining, we can use UiPath's out-of-the-box CustomNamedEntityRecognition ML model (Figure 11.7).

ML Packages / Out of the box Packages / UiPath Language Analysis

UiPath Language Analysis

![](img/05268897c8ac5d80eb3fead1cc8a822a_351_0.png)

Figure 11.7 – UiPath's CustomNamedEntityRecognition out-of-the-box ML model

This ML package allows us to bring our own datasets, pre-tagged (or labeled) with entities we need to extract. This can open the door to many new types of use cases with many different types of entities, such as the following:

- Insurance use case – vehicle entities
- Healthcare – drug entities
- Retail – clothing entities

With this ML model, we need to provide a dataset prelabeled with the entities we want to extract. The training and evaluation datasets need to be in the CoNLL format, which can be performed using a tool named Label Studio.

![](img/05268897c8ac5d80eb3fead1cc8a822a_352_0.png)

Figure 11.8 – Label Studio

Label Studio is an open source tool that allows you to easily label data types such as audio, text, images, and more. With the CustomNamedEntityRecognition ML model, Label Studio will be used to export labeled data into CoNLL 2003 format.

Important Note

For a walk-through on how to use UiPath's Custom NER model, refer to https://docs.uipath.com/ai-fabric/v0/docs/extracting-chemicals-from-research-paper-by-category.

Once the datasets are uploaded into AI Center, the creation of an ML skill is the same as we have performed in previous chapters.

In this section, we introduced the concept of NER, learned how it can be relevant to multiple applicable use case scenarios, and learned about the NER models available in UiPath AI Center. In the next section, we will look into deploying custom Python ML models into AI Center.

## Deploying your own custom models to AI Center

Our examples in previous chapters have all leveraged out-of-the-box ML models developed and trained by UiPath or the open source community. While these out-of-the-box models may fit most use cases, there may be times when you have a custom ML model that you would like to leverage with automation. UiPath AI Center makes it very easy to deploy pre-made Python or AutoML models. In this section, we'll cover the following:

- Preparing a Python model for AI Center
- Deploying to AI Center
- Interacting with UiPath automation

After covering these points, you should have the skills necessary to deploy your own pre-built models into UiPath AI Center.

## Preparing a Python model for AI Center

Before we can deploy a Python model into AI Center, there are a few requirements that we need to add to the Python model. These requirements can be separated into requirements for serving the model and requirements for training the model.

For serving the model, every ML package must contain the following:

- A zipped folder containing a file, `main.py`.
- Within `main.py`, two functions must be implemented:
  - `__init__(self)`: Taking no arguments and loading the model
  - `predict(self, input)`: The function to be called at model serving time, returning a string
- A file named `requirements.txt` with the dependencies needed to run the model.

If we wanted to add re-training and evaluation components to the ML model, the following requirements are necessary:

- A file named `train.py` within the same directory as `main.py`.
- Within `train.py`, the following functions must be implemented:

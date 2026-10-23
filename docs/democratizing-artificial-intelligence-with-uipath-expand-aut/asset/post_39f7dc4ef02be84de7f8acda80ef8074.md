
*Accuracy improving over time:*

**UiPath Document Understanding** also incorporates human validation effort seamlessly with UiPath Action Center and/or the UiPath attended robot. In addition, the corrected data is fed back into the ML to improve accuracy over time. The following diagram shows how UiPath Document Understanding validation and training works:

![](img/05268897c8ac5d80eb3fead1cc8a822a_66_0.png)

Figure 3.11 – UiPath Document Understanding: Validation and training

**UiPath Document Understanding** enables human-bot collaboration in optimizing the result. The models can be retrained automatically based on human input. The more you work with the model, the more effective it becomes.

*End-to-end automation of complex processes:*

**UiPath Document Understanding** is not a standalone product. Combined with the rest of the UiPath platform, it can automate an end-to-end business workflow, as illustrated in the following diagram:

![](img/05268897c8ac5d80eb3fead1cc8a822a_67_0.png)

Figure 3.12 – UiPath Document Understanding: End-to-end business workflow

The end-to-end business workflow allows for maximum efficiency to manage overall workflow and robot utilization. It also prevents external issues from impacting Document Understanding jobs.

## UiPath Document Understanding technical framework

Let's walk through the technical framework of UiPath Document Understanding. You will have a good overview of each component in the framework and how they relate to each other. You will be able to leverage UiPath Document Understanding in automating use cases with a document understanding activity. The following diagram shows the UiPath Document Understanding framework:

![](img/05268897c8ac5d80eb3fead1cc8a822a_68_0.png)

Figure 3.13 – UiPath Document Understanding

The UiPath Document Understanding framework includes five components. They are as follows:

1. **Load Taxonomy**: The first step is to define document types and fields for extraction to create a dedicated taxonomy structure in Taxonomy Manager.
   - **Use Case**: Extract information from travel and entertainment receipts for expense reporting.
   - **Document type**: Receipts
   - **Fields to be extracted**: Transaction date, Vendor, Total amount, Tax amount

2. **Digitize**: The second step is to turn a file into machine-readable content. This step requires the selection of an **Optical Character Recognition (OCR)** engine to digitize the content. UiPath Document Understanding has built-in integration with the most popular OCR engines. The OCR engines are UiPath Document OCR, OmniPage OCR, Google Cloud Vision OCR, Microsoft Azure Computer Vision OCR, Microsoft OCR, Tesseract OCR, and ABBYY Document OCR.

3. **Classify and Validate**: The third step identifies what types of documents the robot is processing. This step leverages AI/ML. There are three sub-steps to this step:
   - **Document classification**: This sub-step automatically determines the relevant document types. There are four OOTB classifiers: Keyword-Based Classifier, Intelligent Keyword Classifier, FlexiCapture Classifier, and Machine Learning Classifier. You can also create your own classifier to implement a custom algorithm.
   - **Document classification validation**: This sub-step allows for human validation using the **Classification Station**. There are two options when it comes to using the Classification Station: as an attended activity or as an Action Center task. This sub-step is optional but highly recommended if you are dealing with multiple document types in a single file or if you need 100% accuracy.
   - **Document classification training**: This sub-step passes the human-validated information back to the classifiers to improve their future predictions. This sub-step closes the feedback loop for classifiers that are capable of learning from human feedback. The available classifier trainers are Keyword-Based Classifier Trainer, Intelligent Keyword Classifier Trainer, and Machine Learning Classifier Trainer.

4. **Extract and Validate**: This step identifies specific information for extraction from the documents. This step leverages AI/ML. There are three sub-steps to this step:
   - **Data extraction**: This sub-step automatically extracts the relevant information from each document type as defined in the taxonomy. This step ensures that the configured extractors are called in the correct order, for the relevant list of fields, and for the specified page range of the file being processed. There are five out-of-the-box extractors: Regex-Based Extractor, Form Extractor, Intelligent Form Extractor, FlexiCapture Extractor, and Machine Learning Extractor. You can also create your own extractor to implement a custom algorithm.

## The benefits of UiPath AI Center

The following diagram shows the benefits of using UiPath AI Center in the UiPath platform:

![](img/05268897c8ac5d80eb3fead1cc8a822a_71_0.png)

Source: UiPath Website

Figure 3.14 – UiPath AI Center: Benefits

UiPath AI Center provides six key benefits:

- OOTB models from UiPath and partners:

**UiPath AI Center** has over 25+ OOTB models from UiPath and many more from the wide network of UiPath partners. These OOTB models cover UiPath Document Understanding, Language Analysis and Comprehension, Image Analysis, and Tabular Data. RPA developers without deep ML backgrounds can start using these OOTB models to incorporate AI into their use cases.

- *Bring your own models:*
**UiPath AI Center** supports users bringing their own ML models into UiPath AI Center to deploy, continuously train, and improve these models. This feature allows RPA developers to collaborate with data scientists in their companies to use the models the data scientists incorporated into their RPA workflows.

- *Human validations:*
**UiPath AI Center** allows humans to work alongside machines to validate data and send it back for model retraining. To maximize automation potential, features that help with human-machine collaboration are essential.

- *Continuous improvement and re-training:*
**UiPath AI Center** enables end-to-end visibility on model use, data, model performance, user actions, and pipelines. It also maintains control over versions and retrains without strain with the human in the loop and automatic retraining.

- *Multiple deployment options:*
**UiPath AI Center** allows for on-premises deployment and cloud deployment. These options satisfy the requirements of all industries.

- *Intuitive and secure data management:*
**UiPath AI Center** has an intuitive, yet secure, data management feature. It is simple to create and access data for the models. It also allows users to upload their own data. In addition, it enables customers to snapshot the data that they trained their model on so that they have a full audit trace of which model made which prediction and which data was trained on which model.

## UiPath AI Center technical concepts

Let’s walk through the technical concepts of **UiPath AI Center**. You will have a good overview of each concept and how they relate to each other. The following diagram shows the technical concepts of UiPath AI Center:

![](img/05268897c8ac5d80eb3fead1cc8a822a_73_0.png)

Figure 3.15 – UiPath AI Center: Core technical concepts

UiPath AI Center uses five technical concepts. You need to be familiar with the concepts and how they relate to each other. They are used throughout the UiPath AI Center product. The five technical concepts are as follows:

- **Project:** A project is a collection of resources – datasets, ML packages, pipeline ML skills, and logs. You use these resources to build a specific ML solution. It is best to create a separate project for each automation use case. Within **UiPath AI Center**, the dashboard provides an overview of all the projects with information on active pipelines and deployed packages.
- **Dataset:** A dataset is a folder of data storage. You can create new data files or upload files from another application. A dataset is a local resource. It is available only within the project.
- **ML Package:** An ML package is a folder with the code and metadata needed to train an ML model. An ML package can have multiple versions with associated change logs. In UiPath AI Center, there are 25+ OOTB ML packages to use.

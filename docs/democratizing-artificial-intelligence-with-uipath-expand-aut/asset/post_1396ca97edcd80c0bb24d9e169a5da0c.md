
- **Checkbox**: The checkbox underneath each classifier simply toggles whether you want to use a classifier for a specific document type. In the following example (*Figure 6.23*), note that **Keyword Based Classifier** is enabled for documents of the **Type1** document type, while **Intelligent Keyword Classifier** is enabled for documents of the **Type2** document type:

![](img/05268897c8ac5d80eb3fead1cc8a822a_154_0.png)

Figure 6.23 – Enabling classifiers for different document types

In this section, we reviewed the Classifiers Wizard with the Keyword Based Classifier and the Intelligent Keyword Classifier. In the next section, we will review the different types of classifiers offered in Document Understanding.

## Reviewing the types of classifiers

Based on the requirements of your use case, you may be drawn to use one or more of the following classifiers available in Document Understanding:

- The Keyword Based Classifier
- The Intelligent Keyword Classifier
- The FlexiCapture Classifier
- The Machine Learning Classifier

When presented with the Classification Station, the user will have the ability to review and correct classifications performed by automation. In the Classification Station, the user can rearrange pages by clicking and dragging pages, and validate classifications performed by classifiers (Figure 6.24):

![](img/05268897c8ac5d80eb3fead1cc8a822a_156_0.png)

Figure 6.24 – Validating the classified invoice using the Classification Station

Important Note
More information on the Classification Station can be found in the UiPath docs: https://docs.uipath.com/activities/docs/present-classification-station#using-classification-station.

With our classification validation completed with the Classification Station, we can leverage our human validation to retrain and strengthen the classifiers used. In the next section, we'll discuss how to train classifiers.

## Training classifiers

One of the great features of the Document Understanding framework is the ability to close the feedback loop and retrain classifiers after validation. Leveraging human input is a great way for automation to teach itself and improve performance for future executions. The training of classifiers is performed through the Train Classifiers Scope activity, as shown in the following screenshot:

## Classifying a document

Let's continue our example with classification.

In this example, we only have one invoice type on a page, so classification is not completely necessary. However, if our input file contains multiple documents (such as multiple invoices), then classification is required:

1. Drag a **Classify Document Scope** activity into the workflow after **Digitize Document**.

Within the **Classify Document Scope** activity, we need to insert our file path, taxonomy, document text, and DOM as input, and assign a variable as output to hold the classification result.

2. Insert the path to the invoice document as the **Document Path** input, and insert our taxonomy, document text, and DOM variables into the **Taxonomy**, **Document Text**, and **Document Object Model** inputs.

3. Create a variable called `classification_results` and insert it into the **Classification Results** output:

![](img/05268897c8ac5d80eb3fead1cc8a822a_158_0.png)

Figure 6.26 – The Classify Document Scope activity before adding a classifier

Since this document is an embedded PDF, we can use the Keyword Based Classifier for classification. The `Sliced Invoices` template appears very standardized, with the text `Sliced Invoices` appearing at the beginning of the document, so we can use that as our keyword to match.

4. Save a blank JSON file called classification.json in the project folder. This JSON file will be used as we manage the learning of the Keyword Based Classifier.

5. Click Manage Learning and choose our recently created classification.json file to start learning.

Within the Keyword Based Classifier learning window, note the document type (Sliced Invoices) from our taxonomy. To start the learning process, we need to include common keywords within the document type. As mentioned earlier, the text Sliced Invoices is consistent with this type of invoice, so let's use that as a keyword.

6. Click Add new keyword set, and add the keywords "sliced invoices" and "sliced invoices" (Figure 6.27):

![](img/05268897c8ac5d80eb3fead1cc8a822a_159_0.png)

Figure 6.27 – Learning the Keyword Based Classifier

Let's next configure our classifier. Configuring our classifiers will be extremely important if we leverage multiple classifiers at once; however, in this case, we are only using one classifier, so we must ensure that it's enabled and that its confidence threshold is set to 0% so that all results from Keyword Based Classifier are returned.

7. Click **Configure Classifiers**. Set the minimum confidence to 0%, and make sure that the checkbox under **Keyword Based Classifier** is filled:

![](img/05268897c8ac5d80eb3fead1cc8a822a_160_0.png)

Figure 6.28 – Setting the Keyword Based Classifier to 0% confidence

With that done, our Classifiers Scope is complete. The next step is checking the confidence of the classifier. In this case, if the confidence returned by our classifier is below a certain percentage, let's have the automation launch the Classification Station to get a more accurate classification from the user.

8. Drag an **If** activity after the Classification Scope, inserting the following as the condition:

```
classification_results(0).Confidence < .5
```

13. Lastly, click on Configure Classifiers, selecting Keyword Based Classifier Trainer for the Sliced Invoices document type (Figure 6.30):

![](img/05268897c8ac5d80eb3fead1cc8a822a_162_0.png)

Figure 6.30 – Saving the Keyword Based Classifier Trainer

And that's it for classifying! Try running the automation. If the Classification Station does not appear, then try increasing the confidence threshold in the If statement. Try also inserting a log message or a message box activity to print the confidence, and note how the confidence increases after each validated run.

Now, we have loaded the taxonomy, digitized text, and classified our invoice document. In the next section, we'll learn about extracting data from digitized text.

## Introducing extraction

Once the classification of our document types is complete, automation is now ready to extract relevant information from each document. With extraction, automation will extract relevant data fields based on the taxonomy created at the beginning of the Document Understanding framework.

## Introducing the Data Extraction Scope

Extraction is performed through the **Data Extraction Scope** activity:

![](img/05268897c8ac5d80eb3fead1cc8a822a_163_0.png)

Figure 6.31 – Data Extraction Scope

Within the **Data Extraction Scope** activity, let's cover the **Input** and **Output** parameters present:

- **Input**:
  - **Document Path**: The full path to our target document
  - **Taxonomy**: The taxonomy returned from the Taxonomy Manager
  - **Classification Result**: The results from classification
  - **Document Text**: The document text returned from digitization
  - **Document Type Id**: The document type ID found in Taxonomy Manager – this field is optional if the classification result is used
  - **Document Object Model**: The DOM returned from digitization

![](img/05268897c8ac5d80eb3fead1cc8a822a_165_0.png)

# Figure 6.33 – Leveraging multiple classifiers at once

Note within each extractor (**Regex Based Extractor** and **Form Extractor** from *Figure 6.33*) the ability to toggle a checkbox to the right of each field within a document type. This enables a hybrid approach of leveraging multiple classifiers per data field, allowing us to set a specific extractor per data field, or allowing us to set multiple extractors as "fallback" extractors if the highest priority extractor returns low confidence.

For testing purposes, the Validation Station should be enabled to help retrain the extractor and classifier, thus both assets should be set to True. Once testing is completed, you may want to turn off validation by setting the assets to False, or by adding additional logic to the workflow to only present the Validation Station when the classifier or extractor returns low confidence. Another recommendation is to work with the business and agree on a success metric – for example, testing 100 scenarios, resulting in a 90% positivity rate, before the Validation Station is disabled.

# Testing with sample receipts

With the Validation Station enabled, we can now test the performance of the deployed classifier and extractor and retrain both skills where necessary. To start testing, navigate to the following link to retrieve sample receipts: https://github.com/PacktPublishing/Democratizing-Artificial-Intelligence-with-UiPath/tree/main/Chapter08/Receipt%20Data.

At the preceding link, you will find receipts to test with and receipts for use after testing is complete. We will be leveraging the receipts located within the Testing folder for the purpose of testing.

In Chapter 7, Testing and Refining Development Efforts, we discussed that when training cognitive automation we should have multiple datasets to test our ML skill:

+   - **Training dataset**: The initial dataset to train the ML model
+   - **Evaluation dataset**: A smaller set of data used to evaluate the training of the ML model
+   - **Testing dataset**: An unbiased dataset used to expose the trained ML model to real-life data

Fortunately, for this use case, the ML skills we used (the **DocumentUnderstanding** Classifier and the **Receipts** Extractor) already come pre-trained and ready to use. That means that UiPath has already performed the initial training and evaluation for us, leaving us to only test the dataset with our unbiased data, retraining with the Validation Station where necessary.

Within the testing folder, you will find 50 receipts to test with. Feel free to test with a subset of the 50 receipts, or to individually test with all 50. When testing, be sure to validate the classification (Figure 8.38) and extraction (Figure 8.39) to further strengthen the ML skills deployed to AI Center:
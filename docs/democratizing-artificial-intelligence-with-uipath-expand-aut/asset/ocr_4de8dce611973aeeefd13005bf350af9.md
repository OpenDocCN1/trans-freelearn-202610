+   9. Open the **40_TrainClassifers.xaml** workflow.
+   10. Within **Train Classifiers Scope**, replace the **Intelligent Keyword Classifier Trainer** activity with a **Machine Learning Classifier Trainer** activity.
+   11. Within the Machine Learning Classifier Trainer, choose the AI Center project (**Note Receipt Details**) and dataset (**DocumentUnderstanding_Classifier**) that we created earlier (*Figure 8.28*):

![](bbox=[0.21, 0.25, 0.74, 0.61])

Figure 8.28 – Setting up the Machine Learning Classifier Trainer

Similar to the Machine Learning Classifier activity, after we add the Machine Learning Classifier Trainer, we must configure the classifier to classify the **Receipt** document type from our taxonomy.
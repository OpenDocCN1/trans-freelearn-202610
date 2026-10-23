+   4. Navigate to **ML Skills** and create a new ML skill using the **Receipts** ML package created earlier.

Once the ML Extractor is deployed, we must next connect it to our UiPath project.

+   5. Navigate back to the **50_Extract.xaml** workflow.

+   6. Within **Data Extraction Scope**, remove the endpoint within the **Receipts ML Extractor** activity and choose the **Receipts** skill underneath the **ML Skill** dropdown (*Figure 8.33*):

![](bbox=[0.13, 0.275, 0.87, 0.645])

Figure 8.33 – The endpoint replaced with the deployed ML skill

With the Receipts extractor deployed and connected to our UiPath project, the last step in extraction is setting up the trainer.

+   7. Open the **60_TrainExtractors.xaml** workflow.

+   8. Within **Train Classifiers Scope**, select the **Note Receipt Details** project and **Receipts** dataset within the **Receipts Extractor Trainer** activity (*Figure 8.34*):
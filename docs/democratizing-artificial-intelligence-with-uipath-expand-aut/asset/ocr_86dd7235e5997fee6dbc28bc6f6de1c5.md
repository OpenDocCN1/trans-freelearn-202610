Once completed, the evaluation pipeline will return a few artifacts as output, as follows:

+   - `Evualation.csv`: Data used to evaluate the model
+   - `Evaluation-report.pdf`: Output report of the pipeline run

If you look at the evaluation report, you can see that pretty good performance can be achieved with relatively little data.

The **key performance indicator (KPI)** to look for in the evaluation report is to achieve a greater-than-80% Fscore for binary classification problems or a greater-than-70% F-score for multi-class problems. In this case, we just reached 80%; however, if we wanted to increase this number, we could increase the size of our dataset or close the feedback loop with a retraining mechanism, which we'll build later.

And that's it! In the next section, we'll deploy the ML skill.

# Deploying the ML skill

Once deployed into AI Center, an ML skill can be consumed by automation. Choosing the **ML Skills** activity in UiPath Studio, you can select from a list of ML skills deployed on AI Fabric, passing input data and leveraging output data as needed.

With our model trained and evaluated, we can now deploy it into a skill usable with **robotic process automation (RPA)**. Proceed as follows:

+   1. Navigate to **ML Skills** by clicking the **ML Skills** tab in the project dashboard.
+   2. Click **Create New** to create a new ML skill, selecting the following:

+   - **Name:** `Email_Spam_Classification`
+   - **Choose Package:** `TextClassification`
+   - **Choose package major version:** 6
+   - **Choose package minor version:** 1
+   - **Enable Auto Update:** Turn this on
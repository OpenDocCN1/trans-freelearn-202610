+   - As is the case with many AI programs, you'll also be looking for ways to automate your processes and workflows. AI/ML technologies are consistently changing and improving, so you'll make a lot of traction when you invest in applying automation for your AI program itself. Use the brainpower of your talented staff to make higher-level decisions and use their complex problem-solving abilities. Anything that can be automated, refreshed, or standardized should be. This aligns with the precepts of agile and lean development. Refactoring and making elegant improvements in your code should be done proactively, so make sure to plan this into your sprints responsibly and use the advantages of your tech stack in your favor to set your team up for success.

While teams will work with R, Java, and C++ for ML, Python is by far the most widely used programming language for ML applications. A major reason for this is that it's a relatively simple language that's rich in libraries and tools to help with ML and data analysis. Python also has libraries like pandas and NumPy for tasks like data processing and manipulation, and libraries for data visualization like Matplotlib and Seaborn. There are also other popular libraries devoted to ML like TensorFlow, PyTorch, scikit-learn, and Keras to further expand on your ML models. Because Python is so widely used, an AI PM should be relatively familiar with it if they're going to work in the private sector.

Managing ML experimentation is a formidable undertaking in and of itself, and we've seen tools such as MLflow, Weights & Biases, and Neptune.ai that are used for managing versions and experiments. You can also use tools such as Cloudera Data Science Workbench, Seldon, Dataiku, DataRobot, Domino, TensorFlow Extended, Kubeflow, and SageMaker to support your data scientists with a workstation for building, experimenting with, deploying, and training ML models. Let's look at some tools that are most useful for complete model management, which includes quite a lot of aspects:

+   - When you're working with ML in production, you need to keep track of the versions of each model, along with that model's code, metadata, and configurations.
+   
+     **Tools:** MLflow, Data Version Control (DVC), and Weights & Biases (W&B).

+   - You're also packaging each trained model into some deployable container for when you're ready to deploy a particular model into production.
+   
+     **Tools:** Docker, TensorFlow Serving, and Seldon.

+   - Because you're often working with so many different models, you're storing all these versions in a centralized repository of some kind so you can easily retrieve different versions.
+   
+     **Tools:** Model Registry (MLflow) and Amazon SageMaker Model Registry.

+   - Depending on the models you're using and the demand for each, you'll need a more automated way of retraining the models you're saving on new data.
+   
+     **Tools:** Kubeflow, Airflow, and Tecton.
The methods for automating feature engineering are often similar: many possible transformations are applied to all existing features, and the usefulness of the generated features is tested after modeling. Examples of how transformations generate features can be found in the following case study.

Another method for automating feature engineering is extracting features using rules based on the data type of the features. For example, the day, week, month, and year could be extracted from a date field. Or the number of characters, lemmas, stems, or embeddings of word or sentence features could be calculated.

As you may notice, the application of feature engineering automation techniques relies on technical information about the features at hand, typically the data type, alongside correlation and relationships with other features. Importantly, there is no domain knowledge applied when creating new features. This highlights one of the shortcomings of AutoML: it cannot handle specific, domain-driven decisions that require human expertise. For example, consider a diabetes dataset with a feature for fasting blood sugar. A medical professional (domain expert) knows that an individual with a fasting blood sugar of 100 to 125 mg/dL is considered prediabetic, and any higher is considered diabetic. This continuous feature can be engineered to specific classes: normal, prediabetic, and diabetic, simplifying the data for modeling. A transformation like this is not possible to achieve with AutoML systems.

# Automating model selection and tuning

Areas where AutoML is particularly useful are model selection and hyperparameter tuning. Given the plethora of algorithms available, choosing the best one for a particular dataset and problem can be daunting. AutoML systems use various techniques, including Bayesian optimization and meta-learning, to select the best model. They also automate the tuning of hyperparameters to maximize model performance.

AutoML systems can provide automated cross-validation, reducing the risk of overfitting and ensuring the model’s generalizability to unseen data. Once the optimal model is selected and trained, many AutoML tools can also help with deployment, making the model available for inference on new data.

Beyond the initial model deployment, some AutoML solutions also provide value in ongoing model monitoring and maintenance. As real-world data evolves, models may suffer from drift, and their performance can degrade. AutoML can help monitor model performance and retrain the model as needed, ensuring that your ML system remains effective in the long run.

# Risks of using AutoML systems

As mentioned previously, AutoML systems typically use no domain knowledge to aid in feature engineering, model selection, or other automation. Instead, a brute-force or scattershot approach of try and see is used.

The “black box” nature of some AutoML systems can also make it challenging to interpret decisions made by the system, making it less suitable for applications that require high levels of explainability.
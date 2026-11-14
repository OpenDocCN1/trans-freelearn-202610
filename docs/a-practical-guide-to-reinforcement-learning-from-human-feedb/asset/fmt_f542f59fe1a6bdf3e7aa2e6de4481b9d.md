
## Model Selection and Baseline Evaluation

Upon preprocessing the data into tokenized dataset, the next step involves Model Selection and using it to inference and test. We will use the `AutoModelForSequenceClassification` class from `transformers` library. This class allows you to automatically load a model suitable for sequence classification based on the model name you provide (e.g., `distilbert-base-uncased`). This provides flexibility to easily switch between different models without worrying about the underlying architecture. It also automatically fetches the configuration associated with that model with details like the number of output classes, and works seamlessly with the corresponding tokenizer. The output of the model includes `logits`, which can be converted to probabilities using a `softmax` function. This is useful for determining the predicted class probabilities for the given input. The model loading is mostly specifying the model and using the `AutoModel` method.

```
# Load the pretrained model
model = AutoModelForSequenceClassification.from_pretrained("distilbert-base-uncased", num_labels=2)
```

To evaluate the models, we write a function to compute some metrics. Here we will use the `accuracy_score` that checks predicted output with `labels` on sentiment score of the movie reviews, then we also use weighted average `f1_score` comparing `labels` and `predictions`:

```
# Evaluation function
def compute_metrics(pred):
    logits, labels = pred
    predictions = np.argmax(logits, axis=-1)
    accuracy = accuracy_score(labels, predictions)
    f1 = f1_score(labels, predictions, average="weighted")
    return {"accuracy": accuracy, "f1": f1}
```

In the snippets below, we are running inference for the 20% test split data, `test_dataset`, we loaded. Since model is pretrained, we are just going to inference using the base model, and consider the evaluation as baseline evaluation to compare with fine-tuning later. The `TrainingArguments` and `Trainer` methods imported from `transformers` library are used for specifying the prediction parameters. Note that we disable logging to `Weights & Biases`, an experimentation framework. But, readers can remove the line `report_to = 'none'` to enable default Weights and Biases experimentation framework. It will require subscription key to use experimentation framework. There are also other opensource versions such as `Mlflow`, which readers can explore later. Usually this is helpful in real projects, where you want to track experimentation progress, as it involves `hyperparameter tuning` based on weights, `batch size`, `tokenization`, and prompts in `GPT` based experiments. For now, we do not require it.

```
# Trainer for evaluation
training_args = TrainingArguments(
    output_dir="./results",
    per_device_eval_batch_size=8,
    report_to='none')                # Disable logging to W&B)
trainer = Trainer(model=model, args=training_args, eval_dataset=test_dataset, compute_metrics=compute_metrics)
# Baseline evaluation
baseline_results = trainer.evaluate()
print("Baseline Results:", baseline_results)
```

Upon running the inference, the output might have a message indicating that some weights were not initialized from model checkpoint. It is okay to disregard it for now. This is mainly for models resuming training from exact weights, but that is not our goal here. This is how the output will appear: Some weights of `DistilBertForSequenceClassification` were not initialized from the model checkpoint at `distilbert-base-uncased` and are newly initialized: `['classifier.bias', 'classifier.weight', 'pre_classifier.bias', 'pre_classifier.weight']`. You should probably `TRAIN` this model on a down-stream task to be able to use it for predictions and inference. The output of the inference, `baseline_results` will appears something like this:

```
Baseline Results: {'eval_loss': 0.6827018857002258, 'eval_model_preparation_time': 0.0025, 'eval_accuracy': 0.765, 'eval_f1': 0.8668555240793201, 'eval_runtime': 3.0755, 'eval_samples_per_second': 65.03, 'eval_steps_per_second': 8.129}
```

We will use these later for comparison after fine-tuning the model using the training data.

### Fine-tuning

Now, we train the pre-trained transformer model (`DistilBERT` in this case as discussed in previous sections) on our specific dataset to improve its performance on a targeted task, such as sentiment classification for the movies dataset from `IMDB`. We start by defining the `training arguments`, specifying key hyperparameters such as the `learning rate` (how quickly the model updates), `batch size` (number of samples processed in each training step), number of `epochs` (full passes through the dataset), and `weight decay` (to prevent overfitting). Next, we initialize a `Trainer` object, which automates the fine-tuning process by combining the model, training arguments, dataset, and a function to calculate evaluation metrics. When we call `trainer.train()`, it iterates through the training data in batches, using the `loss function` to adjust model parameters with each batch, gradually adapting the model weights to better predict labels for the task. During this fine-tuning, the model learns patterns specific to the dataset, which enhances its ability to classify new examples in the same category, and this adapted model is expected to perform better than the baseline pre-trained version.

```
# Fine-tuning arguments
training_args = TrainingArguments(
    output_dir="./results",
    evaluation_strategy="epoch",
    learning_rate=2e-5,
    per_device_train_batch_size=8,
    per_device_eval_batch_size=8,
    num_train_epochs=3,
    weight_decay=0.01,
    report_to='none'
)
# Trainer for fine-tuning
trainer = Trainer(
    model=model,
    args=training_args,
    train_dataset=train_dataset,
    eval_dataset=test_dataset,
    compute_metrics=compute_metrics,
)
# Fine-tuning
trainer.train()
```

The training output from this cell will appear like this:

| Epoch | Training Loss | Validation Loss | Accuracy | F1 |
| --- | --- | --- | --- | --- |
| 1 | No log | 0.001113 | 1.000000 | 1.000000 |
| 2 | No log | 0.000520 | 1.000000 | 1.000000 |
| 3 | No log | 0.000415 | 1.000000 | 1.000000 |

```
TrainOutput(global_step=300, training_loss=0.01779682795206706, metrics={'train_runtime': 134.3684, 'train_samples_per_second': 17.861, 'train_steps_per_second': 2.233, 'total_flos': 317921756774400.0, 'train_loss': 0.01779682795206706, 'epoch': 3.0})
```

### Evaluation

Evaluation of trained models is a necessary step in machine learning. Same is true for fine-tuned models. The process is similar, but later we will discuss some challenges specific to large language model tuning. For evaluation of above fine-tuned model, we use the `evaluate` method of the `trainer` class we just used for fine-tuning and compare with `baseline`.

```
# Evaluation after fine-tuning
fine_tuned_results = trainer.evaluate()
print("Results after Fine-Tuning:", fine_tuned_results)
# Compare baseline and fine-tuned results
print("Improvement in Accuracy:", fine_tuned_results['eval_accuracy'] - baseline_results['eval_accuracy'])
print("Improvement in F1 Score:", fine_tuned_results['eval_f1'] - baseline_results['eval_f1'])
```

The fine-tuning results output and difference in accuracy and F1 score between fine-tuned and baseline is output as shown below:

```
Results after Fine-Tuning: {'eval_loss': 0.00041467114351689816, 'eval_accuracy': 1.0, 'eval_f1': 1.0, 'eval_runtime': 3.3424, 'eval_samples_per_second': 59.837, 'eval_steps_per_second': 7.48, 'epoch': 3.0}
Improvement in Accuracy: 0.235
Improvement in F1 Score: 0.1331444759206799
```

Note that just looking at positive improvement, shows that fine-tuning did help, but we will look at percentage improvement to understand relative impact of fine-tuning.

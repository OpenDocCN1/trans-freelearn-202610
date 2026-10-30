# Overfitting and underfitting

ML models might suffer from two types of fitting issues: **overfitting** and **underfitting**. Overfitting means that your model performs very well in the training data, but cannot be generalized to other datasets, such as testing and, even worse, production data. In other words, if you have an overfitted model, it only works on your training data.

When we are building ML models, we want to create solutions that are able to generalize what they have learned and infer decisions on other datasets that follow the same data distribution. A model that only works on the data that it was trained on is useless. Overfitting usually happens due to the large number of features or the lack of configuration of the hyperparameters of the algorithm.

On the other hand, underfitted models cannot fit the data during the training phase. As a result, they are so generic that they can’t perform well with the training, testing, or production data. Underfitting usually happens due to the lack of good features/ observations or due to the lack of time to train the model (some algorithms need more iterations to properly fit the model).

Both overfitting and underfitting need to be avoided. There are many modeling techniques to work around that. For instance, let’s focus on the commonly used **cross-validation** technique and its relationship with the validation box showed in Figure 1.5.

# Applying cross-validation and measuring overfitting

Cross-validation is a technique where we split the training set into training and validation sets. The model is then trained on the training set and tested on the validation set. The most common cross-validation strategy is known as **k-fold cross validation**, where *k* is the number of splits of the training set.

Using k-fold cross-validation and assuming the value of *k* equals 10, we are splitting the train set into 10 folds. The model will be trained and tested 10 times. On each iteration, it uses nine splits for training and leaves one split for testing. After 10 executions, the evaluation metrics extracted from each iteration are averaged and will represent the final model performance during the training phase, as shown in Figure 1.6:
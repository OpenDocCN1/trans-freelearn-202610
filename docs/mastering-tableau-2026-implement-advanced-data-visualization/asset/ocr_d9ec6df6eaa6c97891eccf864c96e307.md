+   3. Now we split the dataset into four: two test datasets that will help us check how accurate our model is, as well as two training datasets to train the model:

```
X_train, X_test, y_train, y_test = train_test_split(df.drop(['Outcome'], axis = 'columns'),df['Outcome'], test_size = 0.2)
```

> > **Note**
> > If you are interested in more detail and the math behind this code, please check https://scikit-learn.org/stable/modules/generated/
> sklearn.ensemble.GradientBoostingClassifier.html.

+   4. Next, we will load the GradientBoosting model from sklearn and fit our data; then, we can immediately visualize the score, which can be interpreted as the percentage of the total that our model predicted the correct value for:

```
gb = ensemble.GradientBoostingClassifier()
gb.fit(X_train, y_train)
gbscore = gb.score(X_test, y_test)
print('gradient boost: ' + str(round(gbscore*100,2)))
```

+   5. And this is the full code in Jupyter:

```
1 import numpy as np
2 import pandas as pd
3 from sklearn import ensemble
4 from sklearn.model_selection import train_test_split
5 df = pd.read_csv('/Users/marleneimer/Downloads/Diabetes.csv', sep = ';')
6 df = df[['Pregnancies', 'Glucose', 'BloodPressure', 'SkinThickness', 'Insulin', 'BMI', 'Age', 'Outcome']]
7
8 X_train, X_test, y_train, y_test = train_test_split(df.drop(['Outcome'], axis = 'columns'),df['Outcome'],
9 							test_size = 0.2)
10
11 gb = ensemble.GradientBoostingClassifier()
12 gb.fit(X_train, y_train)
13 gbscore = gb.score(X_test, y_test)
14 print('gradient boost: ' + str(round(gbscore*100,2)))
```

gradient boost: 72.08

Figure 14.44: Python script

To conclude this first phase, our model predicted the right outcome in 72.08% of cases, either having diabetes or not having diabetes. This is a fairly good result. To further improve it, we could add more data (in terms of rows), extend our input variables (in terms of columns), or create new features like ratios between variables (hidden information could be emphasized this way). But for now, we can continue working with the model, because eventually, we want to predict the likelihood as a percentage of someone having diabetes. Let's switch over to Tableau:
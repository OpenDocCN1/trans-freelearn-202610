We also check the descriptive statistics of the features and for duplicated rows. Two features are found to have outlier values, age and the number of bank accounts, and need to be cleaned.

Notably, the `Type_of_Loan` field has a comma-separated list, including conjunctions, of the types of loans for each individual. For example “student_loan , mortgage_loan , and personal_loan”. The modeling algorithm could not extract the loan types as part of a string. We have to engineer new fields to enable effective modeling.

# ## Data cleaning

We can now proceed with cleaning the data. In summary, the following issues have to be addressed:

+   - Coercing object columns to numeric columns where appropriate
+   - Handling outliers in the age, number of bank accounts, and monthly balance columns
+   - Engineering new features for the types of loans
+   - Handling missing values and duplicate rows

# ## Coercing to numeric columns

One of the main issues with the dataset is the mixed types found in columns that are supposed to be numeric, but due to error values, pandas interprets them as objects. For example, the `Annual_Income` column contains values such as `100000.0_`, which is then interpreted as a string.

To clean and convert the features to numbers, we first remove character symbols using a regular expression:

```
frame[col] = frame[col].astype(str).str.replace(r'^[\\d.]', '', ,
regex=True)
```

This allows us to use pandas to coerce the column to a numeric feature, turning any errors (empty values) into `np.nan` values:

```
frame[col] = pd.to_numeric(frame[col], errors="coerce")
```

The `Credit_History_Age` feature requires more work from our side. The age is specified using natural language such as “12 years and 3 months.” Here, we use Python string processing to convert the years and months to a floating-point number:

```
def clean_credit_age(age) :
    if age == 'nan' :
        return np.nan
    if not "Years" in age:
        return age
    years, months = age.split(" Years and ")
    months = months.replace(" Months", "")
    return int(years) + int(months) / 12
```
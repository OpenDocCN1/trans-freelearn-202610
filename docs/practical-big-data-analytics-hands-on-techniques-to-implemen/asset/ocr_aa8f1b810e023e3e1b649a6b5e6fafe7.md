```
```r
# In the dataset, the column mass represents the body mass index
# Of the individuals represented in the corresponding row

# mass: Body mass index (weight in kg/(height in m)^2)

# Creating a backup of the diabetes dataframe
diabmiss_orig<- diab

# Creating a separate dataframe which we will modify
diabmiss<- diabmiss_orig

# Saving the original values for body mass
actual <- diabmiss_orig$mass

# Change 91 values of mass to NA in the dataset
diabmiss$mass[10:100] <- NA

# Number of missing values in mass
sum(is.na(diabmiss$mass))

# 91

# View the missing values
diabmiss[5:15,]
```
```

We get the output as follows:

|   | pregnant | glucose | pressure | triceps | insulin | mass | pedigree | age | diabetes |
|---|---:|---:|---:|---:|---:|---:|---:|---:|---|
| 5 | 0 | 137 | 40 | 35 | 168 | 43.1 | 2.288 | 33 | pos |
| 6 | 5 | 116 | 74 | 0 | 0 | 25.6 | 0.201 | 30 | neg |
| 7 | 3 | 78 | 50 | 32 | 88 | 31.0 | 0.248 | 26 | pos |
| 8 | 10 | 115 | 0 | 0 | 0 | 35.3 | 0.134 | 29 | neg |
| 9 | 2 | 197 | 70 | 45 | 543 | 30.5 | 0.158 | 53 | pos |
| 10 | 8 | 125 | 96 | 0 | 0 | NA | 0.232 | 54 | pos |
| 11 | 4 | 110 | 92 | 0 | 0 | NA | 0.191 | 30 | neg |
| 12 | 10 | 168 | 74 | 0 | 0 | NA | 0.537 | 34 | pos |
| 13 | 10 | 139 | 80 | 0 | 0 | NA | 1.441 | 57 | neg |
| 14 | 1 | 189 | 60 | 23 | 846 | NA | 0.398 | 59 | pos |
| 15 | 5 | 166 | 72 | 19 | 175 | NA | 0.587 | 51 | pos |

```
```r
# Test with using the mean, we will set all the missing values
# To the mean value for the column

diabmiss$mass[is.na(diabmiss$mass)] <- mean(diabmiss$mass,na.rm = TRUE)

# Check the values that have been imputed
data.frame(actual=actual[10:100], impute_with_mean=diabmiss$mass[10:100])
```
```
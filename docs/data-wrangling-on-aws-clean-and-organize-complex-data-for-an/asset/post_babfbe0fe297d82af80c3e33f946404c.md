
# Filter data that starts with characters and ends with a single number
```python
cleanse_data=output_data[output_data["description"].str.contains("^[a-zA-Z]*[0-9]$")]
cleanse_data
```

|   | name | description | relative_quality |
|---|---|---|---|
| 1 | Luxury1 | level5,1500 seats,Comfortable seats,Leather se... | 1 |
| 3 | Obstructed1 | level3,100 seats,Comfortable seats though,But ... | 5 |
| 5 | Premium1 | level4-5,500 seats,Comfortable but normal seat... | 2 |
| 7 | Standard1 | level1,5500 seats,Comfortable seats,Leather se... | 3 |
| 9 | standing1 | Level1-5,Comfortable seats,Leather seats!!! | 10 |
| 11 | sub-standard1 | level1,200 seats,Front row , risk of injury!!! | 4 |

**Figure 9.40:** Extracting the mocked-up data records alone for data-cleansing activities

We will then split the data within the description column so that better analysis can be done on the data. We will use the split function on the comma delimiter and get the data into different columns as shown in the following screenshot:

```python
# Extract data from column into different columns
pd.options.mode.chained_assignment = None
cleanse_data["Level_data"]=cleanse_data["description"].str.split(",",n=2).str[0]
cleanse_data["No_of_seats"]=cleanse_data["description"].str.split(",",n=2).str[1]
cleanse_data["Seat description"]=cleanse_data["description"].str.split(",",n=2).str[2]
```

**Identify outliers Remove nulls with valid values**

```python
cleanse_data
```

|   | name | description | relative_quality | Level_data | No_of_seats | Seat description |
|---|---|---|---|---|---|---|
| 1 | Luxury1 | level5,1500 seats,Comfortable seats,Leather se... | 1 | level5 | 1500 seats | Comfortable seats,Leather seats!!! |
| 3 | Obstructed1 | level3,100 seats,Comfortable seats though,But ... | 5 | level3 | 100 seats | Comfortable seats though,But wont have any vis... |
| 5 | Premium1 | level4-5,500 seats,Comfortable but normal seat... | 2 | level4-5 | 500 seats | Comfortable but normal seats, good views for t... |
| 7 | Standard1 | level1,5500 seats,Comfortable seats,Leather se... | 3 | level1 | 5500 seats | Comfortable seats,Leather seats!!! |
| 9 | standing1 | Level1-5,Comfortable seats,Leather seats!!! | 10 | Level1-5 | Comfortable seats | Leather seats!!! |
| 11 | sub-standard1 | level1,200 seats,Front row , risk of injury!!! | 4 | level1 | 200 seats | Front row , risk of injury!!! |

**Figure 9.41:** Verifying the workings of the updated flatten function

Now we have data split into multiple columns but still have data that is not clean enough for final analysis. Let us fix the issue one column at a time for better understanding. The Level_data column provides information on which level of the stadium there are seats of that type available. However, some records have data such as Level1-5, indicating the seats are available in level1, level2, level3, level4, and level5. This is very clear to read to the human mind, but if you want to query the seat types available in level3, this data will not provide correct results. We want to expand the level data wherever applicable.

# Capialize the first letter of the column
```python
cleanse_data["Level_data"]=cleanse_data["Level_data"].str.capitalize()
cleanse_data
```

|   | name | description | relative_quality | Level_data | No_of_seats | Seat description |
|---|---|---|---|---|---|---|
| 1 | Luxury1 | level5,1500 seats,Comfortable seats,Leather se... | 1 | Level5 | 1500 | Comfortable seats,Leather seats!!! |
| 3 | Obstructed1 | level3,100 seats,Comfortable seats though,But ... | 5 | Level3 | 100 | Comfortable seats though,But wont have any vis... |
| 5 | Premium1 | level4-5,500 seats,Comfortable but normal seat... | 2 | Level4,Level5 | 500 | Comfortable but normal seats, good views for t... |
| 7 | Standard1 | level1,5500 seats,Comfortable seats,Leather se... | 3 | Level1 | 5500 | Comfortable seats,Leather seats!!! |
| 9 | standing1 | Level1-5,Comfortable seats,Leather seats!!! | 10 | Level1,Level2,Level3,Level4,Level5 | None | Leather seats!!! |
| 11 | sub-standard1 | level1,200 seats,Front row , risk of injury!!! | 4 | Level1 | 200 | Front row , risk of injury!!! |

**Figure 9.44:** capitalize function to change the first character of the column to caps

However, this doesn't capitalize the values that come after commas. If you want to capitalize every word, then use the **title** function to capitalize all words, as shown here:

```python
# Capitalize the first letter of the column
cleanse_data["Level_data"]=cleanse_data["Level_data"].str.title()
cleanse_data
```

|   | name | description | relative_quality | Level_data | No_of_seats | Seat description |
|---|---|---|---|---|---|---|
| 1 | Luxury1 | level5,1500 seats,Comfortable seats,Leather se... | 1 | Level5 | 1500 | Comfortable seats,Leather seats!!! |
| 3 | Obstructed1 | level3,100 seats,Comfortable seats though,But ... | 5 | Level3 | 100 | Comfortable seats though,But wont have any vis... |
| 5 | Premium1 | level4-5,500 seats,Comfortable but normal seat... | 2 | Level4,Level5 | 500 | Comfortable but normal seats, good views for t... |
| 7 | Standard1 | level1,5500 seats,Comfortable seats,Leather se... | 3 | Level1 | 5500 | Comfortable seats,Leather seats!!! |
| 9 | standing1 | Level1-5,Comfortable seats,Leather seats!!! | 10 | Level1,Level2,Level3,Level4,Level5 | None | Leather seats!!! |
| 11 | sub-standard1 | level1,200 seats,Front row , risk of injury!!! | 4 | Level1 | 200 | Front row , risk of injury!!! |

**Figure 9.45:** title function to capitalize the starting character of every word in a column

Next, let us assume we want to identify the number of seats available on a particular level. The **No_of_seats** column has both a number and a trailing value with *seats*. We can extract only the numeric part of the data and store it in the column. We will use the **extract** function with regular expressions to identify and extract only the numeric part of the data as shown in the following screenshot:

```python
# Extract only numbers from the seat column
import numpy as np
cleanse_data['No_of_seats']=cleanse_data['No_of_seats'].str.extract('(\d+)')
cleanse_data
```

|   | name | description | relative_quality | Level_data | No_of_seats | Seat description |
|---|---|---|---|---|---|---|
| 1 | Luxury1 | level5,1500 seats,Comfortable seats,Leather se... | 1 | level5 | 1500 | Comfortable seats,Leather seats!!! |
| 3 | Obstructed1 | level3,100 seats,Comfortable seats though,But... | 5 | level3 | 100 | Comfortable seats though,But wont have any vis... |
| 5 | Premium1 | level4-5,500 seats,Comfortable but normal seat... | 2 | level4,level5 | 500 | Comfortable but normal seats, good views for t... |
| 7 | Standard1 | level1,5500 seats,Comfortable seats,Leather se... | 3 | level1 | 5500 | Comfortable seats,Leather seats!!! |
| 9 | standing1 | Level1-5,Comfortable seats,Leather seats!!! | 10 | level1,level2,level3,level4,level5 | NaN | Leather seats!!! |
| 11 | sub-standard1 | level1,200 seats,Front row, risk of injury!!! | 4 | level1 | 200 | Front row, risk of injury!!! |

![](img/e28602b44ec813a47df57c96b204412c_327_0.png)

Now the **No_of_seats** column only has numeric values and has **Not a Number** (NaN) value for the standing type. This is correct as standing is not really a seat type. However, we can replace the NaN value with **None** to better represent it as a NULL value to imply that the **No_of_seats** column is not applicable to this seat type.

```python
# Replace Nan to None in the data
cleanse_data=cleanse_data.replace(np.nan, None)
cleanse_data
```

|   | name | description | relative_quality | Level_data | No_of_seats | Seat description |
|---|---|---|---|---|---|---|
| 1 | Luxury1 | level5,1500 seats,Comfortable seats,Leather se... | 1 | level5 | 1500 | Comfortable seats,Leather seats!!! |
| 3 | Obstructed1 | level3,100 seats,Comfortable seats though,But... | 5 | level3 | 100 | Comfortable seats though,But wont have any vis... |
| 5 | Premium1 | level4-5,500 seats,Comfortable but normal seat... | 2 | level4,level5 | 500 | Comfortable but normal seats, good views for t... |
| 7 | Standard1 | level1,5500 seats,Comfortable seats,Leather se... | 3 | level1 | 5500 | Comfortable seats,Leather seats!!! |
| 9 | standing1 | Level1-5,Comfortable seats,Leather seats!!! | 10 | level1,level2,level3,level4,level5 | None | Leather seats!!! |
| 11 | sub-standard1 | level1,200 seats,Front row, risk of injury!!! | 4 | level1 | 200 | Front row, risk of injury!!! |

![](img/e28602b44ec813a47df57c96b204412c_327_1.png)

We can get the sum of seats on Level 5 as seen here. But before that, we have to convert the **No_of_seats** field into an integer type and replace null values with zeroes to sum them up:

```python
cleanse_data['No_of_seats']=cleanse_data['No_of_seats'].fillna(0).astype('int')
cleanse_data[cleanse_data['Level_data'].str.contains('Level5')]['No_of_seats'].sum()
2000
```

![](img/e28602b44ec813a47df57c96b204412c_327_2.png)

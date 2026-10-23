
data from the raw format into an aggregated count of seats per category for each sporting location. This data will be used in a dashboard to visualize the type of seats available across different locations.

Pivot functions provide the capability to achieve similar aggregations. We will join the sport_location table data and seat table data, which is loaded already in output_data DataFrame, using a merge operation. We will explore joining DataFrames further in the data structuring section of this chapter. The pivot table will aggregate the number of seats based on their type and stadium name.

```python
import awswrangler as wr
import pandas as pd
import numpy as np
con=wr.mysql.connect("wranglerbook")
sport_loc=wr.mysql.read_sql_query("SELECT * FROM dms_sample.sport_location", con=con)
pivot_data=pd.merge(output_data,sport_loc,left_on="sport_loc_id",right_on="id",how='inner')
final_data=pivot_data.pivot_table(values="seat_nbr", index="name", margins="True", columns="seat_type", aggfunc="count")
final_data
```

| seat_type | luxury | obstructed | premium | standard | sub-standard | All |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **name** | | | | | | |
| AT&T Park | 1000.0 | NaN | 13000.0 | 23915.0 | 4000.0 | 41915 |
| AT&T Stadium | 1000.0 | 1000.0 | 22999.0 | 42000.0 | 13001.0 | 80000 |
| Angel Stadium | NaN | NaN | 17001.0 | 26482.0 | 2000.0 | 45483 |
| Arrowhead Stadium | 3415.0 | 1000.0 | 16000.0 | 50001.0 | 6000.0 | 76416 |
| Bank of America Stadium | 8000.0 | NaN | 23001.0 | 33418.0 | 11000.0 | 75419 |
| ... | ... | ... | ... | ... | ... | ... |
| US Cellular Field | 5000.0 | 1000.0 | 12614.0 | 18001.0 | 4000.0 | 40615 |

Figure 9.28: Pivot tables to aggregate and convert the dataset into the desired structure

In the preceding example, we could see that there are no obstructed seats in AT&T Park (remember this fictional data, though 😊). However, the seat counts are in a decimal type, which is misleading, as we can’t have partial seats. Let us fix this by converting the data type of the column using the format function as follows:

```python
final_data.head(5).style.format("{:.0f}")
```

| seat_type | luxury | obstructed | premium | standard | sub-standard | All |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **name** | | | | | | |
| AT&T Park | 1000 | nan | 13000 | 23915 | 4000 | 41915 |
| AT&T Stadium | 1000 | 1000 | 22999 | 42000 | 13001 | 80000 |
| Angel Stadium | nan | nan | 17001 | 26482 | 2000 | 45483 |
| Arrowhead Stadium | 3415 | 1000 | 16000 | 50001 | 6000 | 76416 |
| Bank of America Stadium | 8000 | nan | 23001 | 33418 | 11000 | 75419 |

Figure 9.29: Using data formatting and styling options in pandas

## Building an End-to-End Data-Wrangling Pipeline with AWS SDK for Pandas

Now we have a different representation of data that was not available in the original raw data source. Similarly, you can convert data based on your project consumption needs using pandas operations such as pivot, unpivot, and so on.

Pandas provides capabilities for structuring your data by removing unwanted columns, renaming columns, and also changing the index as needed. This helps in changing the data format to a more standard format that is suitable for each use case. In addition, there are functions that we can utilize in Pandas for flattening nested data within a single column to better analyze the data.

In order to demonstrate our work, let us consider a sample stadium data where the address and dining options from the stadium are available. We do not have the data in our sample dataset and hence will use a sample value to show how JSON data flattening works. We will use the `json_normalize` (https://pandas.pydata.org/docs/reference/api/pandas.json_normalize.html) function to flatten the nest JSON data into columns for analysis. Now we can use this data to query the columns much more efficiently. You can query how many stadiums are available in the state of California by filtering by the Location.State column.

```python
stadium_address = {'Name': 'Angel Stadium', 'Location': {'City': 'Anaheim', 'Zipcode': '92806', 'State': 'CA', 'Address': {'Street Name': 'E Gene Autry Way', 'Number': '2000'}}, 'Dining': ['The Sweet shop', 'Big A Burger']}
```

```python
from pandas import json_normalize
flat1 = json_normalize(stadium_address)
```

| | Name | Dining | Location.City | Location.Zipcode | Location.State | Location.Address.Street Name | Location.Address.Number |
|:---|:---|:---|:---|:---|:---|:---|:---|
| 0 | Angel Stadium | [The Sweet shop, Big A Burger] | Anaheim | 92806 | CA | E Gene Autry Way | 2000 |

Figure 9.30: Flattening hierarchical JSON data using the json_normalize function

However, we still have data in a list in the Dining column. If we wanted to have that column expanded, it would help us with our analysis further. For this, we need to create a function that will flatten both the dictionary and list data from a JSON string.

```python
# Function referenced from https://towardsdatascience.com/flattening-json-objects-in-python-f5343c794b10
def flatten_json(y):
    out = {}
    def flatten(x, name=''):
        if type(x) is dict:
            for a in x:
                flatten(x[a], name + a + '_')
        elif type(x) is list:
            i = 0
            for a in x:
                flatten(a, name + str(i) + '_')
                i += 1
        else:
            out[name[:-1]] = x
    flatten(y)
    return out
```

Figure 9.31: Function to flatten both the list and dictionary columns with JSON data

Now, you can see the output has both dictionary and list data being flattened from the JSON string. We have used the `flatten_json` function that we created in the preceding step to achieve the desired output:

```python
flat = flatten_json(stadium_address)
json_normalize(flat)
```

Figure 9.32: Flattening JSON data with list and dictionary columns

| | Name | Location_City | Location_Zipcode | Location_State | Location_Address_Street_Name | Location_Address_Number | Dining_0 | Dining_1 |
|:---|:---|:---|:---|:---|:---|:---|:---|:---|
| 0 | Angel Stadium | Anaheim | 92806 | CA | E Gene Autry Way | 2000 | The Sweet shop | Big A Burger |

Let’s take a moment and understand how the function works so that you can develop it further. We will add comments to the function and demonstrate the working of the `flatten` function. The function will be called recursively to flatten the JSON string:

```python
# For learning the working of the function.
def flatten(x, name='', out={}):
    print(x)
    a=""
    if type(x) is dict:
        for a in x:
            print("")
            print("Dictionary route " + a)
            flatten(x[a], name + a + '_')
    elif type(x) is list:
        i = 0
        for a in x:
            print("********************")
            print("List route " + name + str(i) + '_')
            flatten(a, name + str(i) + '_')
            i += 1
    else:
        print("===> Found a value for target column --> " + x)
        out[name[:-1]] = x
```

Figure 9.33: Understanding the working of the flatten function by adding comments

We have added the print statement immediately in the function to understand the input string that is being passed. We have added print statements to each of the sections, which perform the following functions:

- Flattening dictionary objects
- Flattening list objects
- Identifying an atomic value to be stored in the target column

### How does the function work?

The function will iterate through the provided data and identify whether the current object is a dictionary or list:

- A dictionary object will have curly brackets and have a key-value pair structure, for example: `{ "name":"Angel stadium", "state":"CA" }`. Dictionaries can have dictionary or list objects within them.
- List objects will have similar data items that are repeated multiple times. They are stored within a square bracket structure. They can be used to store multi-valued attributes. In the preceding example, we stored data about multiple restaurants within the Angel stadium in a list – for example: `[ "The sweet shop", "Big A burger" ]`.

Once it identifies the object, it will call the function recursively if it’s a dictionary or list within another object.

The following screenshot provides an illustration of how the function parses the data before providing the final output. Follow through each step and you can better understand the workings of this function:

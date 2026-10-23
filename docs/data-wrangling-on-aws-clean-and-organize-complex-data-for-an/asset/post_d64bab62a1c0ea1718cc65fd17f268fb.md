
## Flatten Function Execution Output

```python
flatten(stadium_address)

{'Name': 'Angel Stadium', 'Location': {'City': 'Anaheim', 'Zipcode': '92806', 'State': 'CA', 'Address': {'Street Name': 'E Gene Autry Way', 'Number': '2000'}}, 'Dining': ['The Sweet shop', 'Big A Burger']}

Dictionary route Name
Angel Stadium
====> Found a value for target column --> Angel Stadium

Dictionary route Location
{'City': 'Anaheim', 'Zipcode': '92806', 'State': 'CA', 'Address': {'Street Name': 'E Gene Autry Way', 'Number': '2000'}}

Dictionary route City
Anaheim
====> Found a value for target column --> Anaheim

Dictionary route Zipcode
92806
====> Found a value for target column --> 92806

Dictionary route State
CA
====> Found a value for target column --> CA

Dictionary route Address
{'Street Name': 'E Gene Autry Way', 'Number': '2000'}

Dictionary route Street Name
E Gene Autry Way
====> Found a value for target column --> E Gene Autry Way

Dictionary route Number
2000
====> Found a value for target column --> 2000

Dictionary route Dining
['The Sweet shop', 'Big A Burger']
*********************
List route Dining_0_
The Sweet shop
====> Found a value for target column --> The Sweet shop
*********************
List route Dining_1_
Big A Burger
====> Found a value for target column --> Big A Burger
```

![](img/e28602b44ec813a47df57c96b204412c_319_0.png)

Now, we have successfully flattened the data into individual columns for better analysis. However, the list items are split into multiple columns in the same table, which could create wide column tables and sparse data, which is not good for querying the data. The better approach would be to split the data from the list into a separate table with foreign key relationships.

In order to achieve this, we will change our `flatten` function further. The following is a summary of changes made to the `flatten` function:

- Add logic to handle the primary key for the dataset so that the primary key can be stored in the child table to join them together.
- We will change the flattening logic for list objects to create a different DataFrame. The logic also will populate the primary key iterator column to identify different values from the original list object.
- Now we will have more than one dictionary item returned from our function, so we will change the output of the function to return a list of dictionaries/list objects.

```python
# For learning the working of the function.
def flatten_v2(x, name='', df_name='df', pk='Name', pk_val='', out={}, out_dfs={}):
    print("Input -->", x, "--", df_name, "--", name)
    a = ""
    df1 = {}
    df_list = []
    if pk_val == "":
        pk_val = x[pk]
    if type(x) is dict:
        for a in x:
            print("________________________")
            print("Dictionary route " + a)
            flatten(x[a], name + a + '_', "df", pk_val=pk_val)
    elif type(x) is list:
        i = 0
        for a in x:
            df1 = {}
            print("**************************")
            print("List route " + name + str(i) + '_')
            df_name = name[:-1]
            df1[name[:-1]] = a
            df1[name[:-1] + "_PK"] = i
            df1[pk] = pk_val
            df_list.append(df1)
            print("Output in list-->", df_list)
            i += 1
        out_dfs[df_name] = df_list
        return out_dfs
    else:
        print("====> Found a value for target column --> " + x)
        out[name[:-1]] = x
        out_dfs[df_name] = out
        print("Output-->", out)
        return out_dfs
```

Figure 9.35: Updated flatten function to split list objects into another DataFrame with a primary foreign key relationship

Explore the function and you can execute it on your own environment to understand the inner working better. Let us now bundle this into a working flatten function and verify the output:

```python
def flatten_json_v2(y):
    out = {}
    out_dfs = {}

    def flatten(x, name='', pk_name='df', pk=pk_val):
        df1 = {}
        df_list = []
        if pk_val != '':
            pk_val = pk[pk]
        if type(x) is dict:
            for a in x:
                flatten(x[a], name + ' ', 'df', pk_val=pk, df1=df1, df_list=df_list)
        elif type(x) is list:
            i = 0
            for a in x:
                df1 = {}
                print("**************************")
                print("List route " + name + str(i) + '_')
                df_name = name[:-1]
                df1[df_name] = a
                df1[df_name + "_PK"] = i
                df1[pk] = pk_val
                df_list.append(df1)
                i += 1
            out_dfs[df_name] = df_list
        else:
            out[name] = x
            out_dfs[name] = out
        return out_dfs

    pk_val = y['Name']
    flatten(y, 'Name_', 'df', pk=pk_val)
    return out_dfs
```

Figure 9.36: Updated flattening code created as version 2 of the flatten function

Execute the function for our sample dataset here and we can now see that the function creates two DataFrames with one DataFrame having the contents from the list object along with the primary key value (the stadium name in the sample use case here) for joining purposes.

```python
flat = flatten_json_v2(stadium_address)
json_normalize(flat['df'])
```

| | Name | Location_City | Location_Zipcode | Location_State | Location_Address_StreetName | Location_Address_Number |
|---|------|---------------|------------------|----------------|-----------------------------|-------------------------|
| 0 | Angel Stadium | Anaheim | 92806 | CA | E Gene Autry Way | 2000 |

```python
json_normalize(flat['Dining'])
```

| | Dining | Dining_pk | Name |
|---|--------|-----------|------|
| 0 | The Sweet shop | 0 | Angel Stadium |
| 1 | Big A Burger | 1 | Angel Stadium |

Figure 9.37: Verifying the workings of the updated flatten function

We have gone through this exercise to showcase that with pandas and Python, we can perform powerful data transformation to suit our needs. However, this logic can be further developed and fine-tuned.

### Challenge

Can you also update the script to process nested lists? One example could be that a stadium has a list of food outlets and each food outlet has a list of menu items.

### Hint

You can try to store the primary key separately for each table in the hierarchy and then add them to the child table along with the iterator column specific to that table.

In the next section, we will learn more about using pandas’ operations for performing data cleaning activities.

## Data cleaning

Data cleaning is an important step in the process of data wrangling. A good amount of time is spent on identifying the right data source and cleaning the data. Pandas provides a lot of functionalities for cleaning your data.

The exact activities that are required during this phase are different for each type of dataset. Certain data sources will have data that requires only minimal cleaning and certain other data sources might require a lot of cleaning activities before the dataset can be used in your project. You could also use the output of data exploration activities to understand the level of cleaning activities to be performed on the data.

## Data cleansing with Pandas

In order to demonstrate the data cleaning steps, we will use the `seat_type` table from our database. This table only has minimal data volume, so we will insert some data before we proceed with data cleaning.

The data in `seat_type` looks like the screenshot here. It has three columns for the seat type name, a description of that seat type, and a column that provides a relative ranking of the quality of that seat type:

```python
con = wr.mysql.connect("wranglerbook")
cursor = con.cursor()
cursor.execute("show columns from seat_type")
cursor.fetchall()
```

```
[('name', 'varchar(15)', 'NO', 'PRI', None, ''),
 ('description', 'varchar(120)', 'YES', '', None, ''),
 ('relative_quality', 'tinyint', 'YES', '', None, '')]
```

```python
cursor.execute("select * from seat_type;")
cursor.fetchall()
```

```
[('luxury', 'Excellent seats – box seats, behind the plate, etc. etc.', 1),
 ('obstructed', 'Pretty crappy, partially obstructed seats', 5),
 ('premium', 'Really good seats – first level, mid field, etc. etc.', 2),
 ('standard', 'Standard seats – not super awesome but pretty good', 3),
 ('standing', "Really? That's – not a seat at all now, is it?", 10),
 ('sub-standard', 'End zone, nose bleed, etc.', 4)]
```

Figure 9.38: Exploring the current data available in the seat table

We will modify the data to emulate a dataset that will help us walk through the data-cleaning activities. Rather than deleting or updating the current data from the seat table, we will insert new records that are a mocked-up version of the current data.

The following screenshot shows the new mocked-up records with the same columns in the `seat_type` table. However, the description column has now additional details such as the level of the stadium in which the seat type is available and how many seats of that particular type are available, along with a description.

```python
cursor.execute("insert into seat_type values('Luxury1', 'level1,1500 seats,Comfortable seats,Leather seats!!!',1)")
cursor.execute("insert into seat_type values('Obstructed1', 'level3,100 seats,Comfortable seats though,But wont have any v...',1)")
cursor.execute("insert into seat_type values('Premium1', 'level4-5,500 seats,Comfortable but normal seats, good views for 1...',1)")
cursor.execute("insert into seat_type values('Standard1', 'level1,5500 seats,Comfortable seats,Leather seats!!!',3)")
cursor.execute("insert into seat_type values('standing1', 'level1-5,Comfortable seats,Leather seats!!!',10)")
cursor.execute("insert into seat_type values('sub-standard1', 'level1,200 seats,Front row , risk of injury!!!',4)")
```

Figure 9.39: Mocked-up records for the seat table

For our current use case, we will extract only the data that we inserted into the table. We will filter records that have a seat type name column with values that begin with a letter (a-z) and end with a numeric value (0-9), which will only get the newly inserted records that have the number 1 appended at the end of the name column.

```
output_data.wr.s3.read_json("s3://wranglerbook/aws-wrangler/nfl_data/", lines=True)
output_data.head(2)
```

position  player_number  name        status start1 stat1_val stat2 stat2_val stat3 stat3_val stat4 stat4_val team
0  OT  66.0 Graf, Kevin  ACT      G       --   GS     --  None     None     None  None     INO
1  FS  32.0 Green, T.J.  ACT      TCCL    --   SCK    --  FF       --       INT   --     IND

![](bbox=[0.15, 0.245, 0.87, 0.265])

Once data is loaded into pandas DataFrame, you can use pandas data exploration functionalities to analyze your data. Depending on the type of data and business usage, different types of explorations can be performed. We will explain some high-level data exploration activities here and identify the structure of the data including record count, column count, column types, memory usage, and so on using the info function. This gives you an idea of the data and its structure.

```
output_data.info()

<class 'pandas.core.frame.DataFrame'>
RangeIndex: 2928 entries, 0 to 2927
Data columns (total 13 columns):
#     Column          Non-Null Count  Dtype
---     ------          --------------  -----
0      Position        2928 non-null   string
1      player_number   2916 non-null   Int64
2      Name            2928 non-null   string
3      status          2928 non-null   string
4      stat1           2928 non-null   string
5      stat1_val       2928 non-null   string
6      stat2           2928 non-null   string
7      stat2_val       2928 non-null   string
8      stat3           2390 non-null   string
9      stat3_val       2390 non-null   string
10     stat4           2390 non-null   string
11     stat4_val       2390 non-null   string
12     team            2928 non-null   string
dtypes: Int64(1), string(12)
memory usage: 300.4 KB
```

![](bbox=[0.15, 0.785, 0.87, 0.805])

If you are only interested in the number of records and the number of columns in the DataFrame, you could use the shape function. The usage of the function is data frame .shape .
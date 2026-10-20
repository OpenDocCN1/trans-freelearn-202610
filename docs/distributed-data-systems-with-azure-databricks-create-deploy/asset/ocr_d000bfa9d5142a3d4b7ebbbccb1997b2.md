In a very similar way, we can create a Koalas Series:

```
koala_series = ks.Series([1, 10, 5, np.nan, 10, 2])
```

As was mentioned before, we can create a Koalas Series by passing the previously created pandas Series:

```
koala_series = ks.Series(pandas_series)
```

We can also create it using the from_pandas function:

```
koala_series = ks.from_pandas(pandas_series)
```

One thing to bear in mind is that unlike pandas, Koalas will not guarantee maintaining the order of indices, because of the distributed nature of the Koalas API. Nevertheless, we can use Series.sort_index() to keep the ordered indices, as shown in the next example, where we apply sort_index to a Koalas Series:

```
koala_series.sort_index()
```

A Koalas DataFrame can be created in similar ways as a pandas DataFrame. For example, we can create a Koalas DataFrame by passing a NumPy array, the same way we would do for a pandas DataFrame.

To create a pandas DataFrame from a NumPy array, we can do the following:

```
pandas_df = pd.DataFrame({'val1': np.random.rand(5),
'val2': np.random.rand(5)})
```

Very similarly, we can do the same to create a Koalas DataFrame from a NumPy array:

```
koalas_df = ks.DataFrame({'val1': np.random.rand(5),
'val2': np.random.rand(5)})
```

We can also create a Koalas DataFrame by passing a pandas DataFrame as an argument to the Koalas DataFrame method:

```
koalas_df = ks.DataFrame(pandas_df)
```

Or we can use the from_pandas function:

```
koalas_df = ks.from_pandas(pandas_df)
```
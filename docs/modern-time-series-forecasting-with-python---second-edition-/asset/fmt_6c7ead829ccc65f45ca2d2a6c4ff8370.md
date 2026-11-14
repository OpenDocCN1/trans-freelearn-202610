
# Data Processing and Storage

## Preprocessing Steps

There are two steps we need to do before we can start processing the data into either compact or expanded form:

1.  `Find the Global End Date`: We must find the maximum date across all the block files so that we know the global end date of the time series.

2.  `Basic Preprocessing`: If you remember how `hhblock_dataset` is structured, you will remember that each row had a date and that along the columns, we have half-hourly blocks. We need to reshape that into a long form, where each row has a date and a single half-hourly block. It's easier to handle that way.

Now, let's define separate functions for converting the data into compact and expanded forms and `apply()` that function to each of the `LCLid` columns. We will do this for each `LCLid` separately since the start date for each `LCLid` is different.

## Expanded Form

The function for converting into expanded form does the following:

1.  Finds the start date.
2.  Create a standard `DataFrame` using the start date and the global end date.
3.  Left merges the `DataFrame` for `LCLid` to the standard `DataFrame`, leaving the missing data as `np.nan`.
4.  Returns the merged `DataFrame`.

Once we have all the `LCLid` `DataFrame`s, we must perform a couple of additional steps to complete the expanded form processing:

1.  Concatenate all the `DataFrame`s into a single `DataFrame`.
2.  Create a column called `offset`, which is the numerical representation of the half-hour blocks; for example, `hh_3` → `3`.
3.  Create a timestamp by adding a 30-minute offset to the day and dropping the unnecessary columns.

For one block, this representation takes up ~47 MB of memory.

## Compact Form

The function for converting into compact form does the following:

1.  Finds the start date and time series identifiers.
2.  Creates a standard `DataFrame` using the start date and the global end date.
3.  Left merges the `DataFrame` for `LCLid` to the standard `DataFrame`, leaving the missing data as `np.nan`.
4.  Sorts the values on the date.
5.  Returns the time series array, along with the time series identifier, start date, and the length of the time series.

Once we have this information for each `LCLid`, we can compile it into a `DataFrame` and add `30min` as the frequency.

For one block, this representation takes up only ~0.002 MB of memory.

We are going to use the compact form because it is easy to work with and much less resource hungry.

## Mapping Additional Information

From the data model that we prepared earlier, we know that there are three key files that we have to map: *Household Information*, *Weather*, and *Bank Holidays*.

### Household Information

The `informations_households.csv` file contains metadata about the household. There are static features that are not dependent on time. For this, we just need to left merge `informations_households.csv` to the compact form based on `LCLid`, which is the time series identifier.

> **BEST PRACTICE**
>
> While doing a pandas merge, one of the most common and unexpected outcomes is that the number of rows before and after the operation is not the same (even if you are doing a left merge). This typically happens because there are duplicates in the keys on which you are merging. As a best practice, you can use the `validate` parameter in the pandas merge, which takes in inputs such as `one_to_one` and `many_to_one` so that this check is done while merging and will throw an error if the assumption is not met. For more information, go to [https://pandas.pydata.org/docs/reference/api/pandas.merge.html](https://pandas.pydata.org/docs/reference/api/pandas.merge.html)

### Bank Holidays

Bank Holidays and Weather, on the other hand, are time-varying features and should be dealt with accordingly. The most important aspect to keep in mind is that while we map this information, it should perfectly align with the time series that we have already stored as an array.

`uk_bank_holidays.csv` is a file that contains the dates of the holidays and the kind of holiday. The holiday information is quite important here because the energy consumption patterns would be different on a holiday when the family members are at home spending time with each other or watching television, and so on. Follow these steps to process this file:

1.  Convert the date column into the datetime format and set it as the index of the `DataFrame`.
2.  Using the `resample()` function we saw earlier, we must ensure that the index is resampled every 30 minutes, which is the frequency of the time series.
3.  Forward fill the holidays within a day and fill in the rest of the `NaN` values with `NO_HOLIDAY`.

Now, we have converted the holiday file into a `DataFrame` that has a row for each 30-minute interval. On each row, we have a column that specifies whether that day was a holiday or not.

### Weather

`weather_hourly_darksky.csv` is a file that is, once again, at the daily frequency. We need to downsample it to a 30-minute frequency because the data that we need to map to this is at a half-hourly frequency. If we don't do this, the weather will only be mapped to the hourly timestamps, leaving the half-hourly timestamps empty.

The steps we must follow to process this file are also similar to the way we processed holidays:

1.  Convert the date column into the datetime format and set it as the index of the `DataFrame`.
2.  Using the `resample()` function, we must ensure that the index is resampled every 30 minutes, which is the frequency of the time series.
3.  Forward fill the weather features to fill the missing values that were created while resampling.

Now that you have made sure the alignment between the time series and the time-varying features is ensured, you can loop over each of the time series and extract the weather and bank holiday array before storing it in the corresponding row of the `DataFrame`.

## Saving and Loading Files to Disk

The fully merged `DataFrame` in its compact form takes up only ~10 MB. But saving this file requires a little bit of engineering. If we try to save the file in CSV format, it will not work because of the way we have stored arrays in pandas columns (since the data is in its compact form). We can save it in `pickle` or `parquet` format, or any of the binary forms of file storage. This can work, depending on the size of the RAM available in our machines. Although the fully merged `DataFrame` is just ~10 MB, saving it in `pickle` format will make the size explode to ~15 GB.

What we can do is save this as a text file while making a few tweaks to accommodate the column names, column types, and other metadata that is required to read the file back into memory. The resulting file size on disk still comes out to ~15 GB but since we are doing it as an I/O operation, we are not keeping all that data in our memory. We call this the time series (`.ts`) format. The functions for saving a compact form in `.ts` format, reading the `.ts` format, and converting the compact form into expanded form are available in this book's GitHub repository under `src/data_utils.py`.

If you don't need to store all of the `DataFrame` in a single file, you can split it into multiple chunks and save them individually in a binary format, such as `parquet`. For our datasets, let's follow this route and split the whole `DataFrame` into chunks of blocks and save them as `parquet` files. This is the best route for us because of a few reasons:

*   It leverages the compression that comes with the format.
*   It reads in parts of the whole data for quick iteration and experimentation.
*   The data types are retained between the read and write operations, leading to less ambiguity.

> **NOTE**
>
> For very large datasets, we can use some pandas alternatives which makes it easier to process datasets which are out of memory. `Polars` is a great library which has lazy loading and is very fast. And for truly huge datasets, `pyspark` with a distributed cluster might be the right choice.

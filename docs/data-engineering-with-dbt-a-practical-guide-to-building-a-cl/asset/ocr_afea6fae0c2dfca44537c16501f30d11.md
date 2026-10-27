In the preceding code, we have done the following:

+   - Added the `test:` property under the table name and the `unique_combination_of_columns` test, passing the two columns that we want (ACCOUNTID and SYMBOL) as a YAML list.

+   - Added a pseudo-column that in place of the name has a valid SQL expression.

This is always valid, and by using the `CONCAT_WS` function, you can concatenate as many columns as you want using a separator. As you can see, this has the advantage of being quick and easy to read and places the test in the same place as the other column tests. Just be aware that the concatenation is not strictly needed and might add a lot of overhead, with big tables. The table test just does a `group` by of the columns to test, which is the minimum required and fast. It also places the test directly under the table making it to stand out and avoiding to mix the PK test with the tests on the other columns.

+   - Removed the `unique` test from the SYMBOL column.

To recap, we can say that it is fine to use the `CONCAT_WS` function on small tables while using the test from `dbt_utils` package when tables are big (100 million+ rows). We should consider moving such important tests to the next layer only when we need to edit or extract the columns involved or filter the source.

# Testing a CSV seed

Our second input is a CSV file that we load as `seed`.

In this case, we have the possibility of doing our tests in three places:

+   - **On the CSV seed:** We would place a test here if the requirement or importance of the test relates to the source being a CSV file. An example could be parsing dates or numbers, or in our case, correctly applying a post-hook query.

+   - **On the seed source table:** We would place a test here if the test was important independent of the type of input. In other words, it relates to the semantics of the data.

+   - As always, we could pass the test forward in the STG model, but we should prefer to test earlier if possible.

To show you both ways, let’s add a `not null` test for the `LOAD_TS` column on the seed and any other tests as we see fit on the source, which we have defined to read the CSV.

Let’s start with the seeds:

+   1. Create a `seeds.yml` file under the `seeds` folder

+   2. Add the following code:

```
version: 2
seeds:
```
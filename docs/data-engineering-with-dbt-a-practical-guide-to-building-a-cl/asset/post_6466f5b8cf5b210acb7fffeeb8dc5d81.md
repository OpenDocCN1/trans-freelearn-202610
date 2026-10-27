
In the preceding code, we have done the following:

- Added the `test:` property under the table name and the `unique_combination_of_columns` test, passing the two columns that we want (ACCOUNTID and SYMBOL) as a YAML list.

- Added a pseudo-column that in place of the name has a valid SQL expression.

This is always valid, and by using the `CONCAT_WS` function, you can concatenate as many columns as you want using a separator. As you can see, this has the advantage of being quick and easy to read and places the test in the same place as the other column tests. Just be aware that the concatenation is not strictly needed and might add a lot of overhead, with big tables. The table test just does a `group` by of the columns to test, which is the minimum required and fast. It also places the test directly under the table making it to stand out and avoiding to mix the PK test with the tests on the other columns.

- Removed the `unique` test from the SYMBOL column.

To recap, we can say that it is fine to use the `CONCAT_WS` function on small tables while using the test from `dbt_utils` package when tables are big (100 million+ rows). We should consider moving such important tests to the next layer only when we need to edit or extract the columns involved or filter the source.

## Testing a CSV seed

Our second input is a CSV file that we load as `seed`.

In this case, we have the possibility of doing our tests in three places:

- **On the CSV seed:** We would place a test here if the requirement or importance of the test relates to the source being a CSV file. An example could be parsing dates or numbers, or in our case, correctly applying a post-hook query.

- **On the seed source table:** We would place a test here if the test was important independent of the type of input. In other words, it relates to the semantics of the data.

- As always, we could pass the test forward in the STG model, but we should prefer to test earlier if possible.

To show you both ways, let’s add a `not null` test for the `LOAD_TS` column on the seed and any other tests as we see fit on the source, which we have defined to read the CSV.

Let’s start with the seeds:

1. Create a `seeds.yml` file under the `seeds` folder

2. Add the following code:

```
version: 2
seeds:
tests:
  - not_null
  - not_empty
```

In the preceding code, we have done the following:

- Tested that the SECURITY_CODE column is unique and not null, implicitly defining it as a business key
- Tested that the SECURITY_NAME column is not null and not empty; we will use the security name very often, and we cannot accept that it is empty

We leave it to you as an exercise to add more not_null and/or not_empty tests for other columns of the ABC_Bank_SECURITY_INFO table if you consider this to be important.

## Adding a test to avoid a regression

If we look at our STG layer, we do not have much left to test, as we have managed to do the important tests (unicity and mandatory not null fields) on the source and seed.

There has been only one field that we needed to transform, the report date that was stored with only two digits in the CSV file and was loaded as a date in the 1st century.

We have decided to keep the load unchanged (getting a date) and to “fix” the date in our STG model, creating a macro. We could have changed the load to load a string from the CSV and parse it correctly as a two-digit year in the STG model.

Both work, and we cannot exclude that someone down the line will dislike our solution and change the load or find another way, including getting the data from a different source. What we can do now to improve the quality and trust in our load is to put in place a test to avoid regressing to a problem that we have already solved.

Let’s add a test to check that the REPORT_DATE column is in the 21st century:

1. Create a staging.yml file in the models/staging folder.
2. Add the following code to the file:

```
version: 2
models:
  - name: STG_ABC_BANK_POSITION
    tests:
      - dbt_utils.expression_is_true:
          expression: " YEAR(REPORT_DATE) >= 2000 "
```

We will use the expression_is_true test from the dbt_utils package to express the table-wide condition that the year of the REPORT_DATE column is greater than 2000, or any value that’s right for you (maybe 1970 or 1900 if you have older data).

This is a very simple and efficient way to perform unit tests of your macros, but it is only one way, and many variations of this pattern can be used, depending on the test to be performed.

You can provide the input data in multiple ways, with a simple SELECT and UNION ALL as we did, but if you plan to provide a bigger amount of data, you might want to move to a SELECT FROM VALUES SQL clause or even have the data in a proper table, either loaded as CSV seed or as source provided to you and accessed for the test.

In some cases, it might be enough to verify that the results satisfy a rule instead of expressing the expectation with a value for each input, so you might not have the expected data, as it will be replaced by a calculation following the given rule.

In other cases, the expected data might not be tied to an individual row, but you will have an expected result set. In such a case, you will get two tables, one for the input data and one for the expected data, and you can use the MINUS (same as EXCEPT) set operator:

```
SELECT results
MINUS
SELECT expectations
UNION
SELECT expectations
MINUS
SELECT results
```

The first part of the preceding query verifies that all the rows in the results are expected rows from the expectation table, while the second part verifies that only the expected rows from the expectation table are produced in the results. The two parts together verify that the result produces the same rows as in the expectations.

We have already applied the no_hash_collisions test to our HIST models when we introduced the generic tests, so we do not need any more tests on them.

## Avoiding orphan keys in fact tables

Moving to the refined layer, we have already in place a couple of tests that ensure that our calculated values, our BRs, are not null.

Our two REF models are also so simple that they do not do any join, so the only useful thing that we have left to test is that the relationship between the position fact and the security dimension is well formed and that the fact does not have orphans.

Let’s use the relationship test provided by dbt to ensure that:

1. Open the refined.yml file in the refined folder.
2. Add the following code:

```
- name: REF_POSITION_ABC_BANK
  columns:
    ...
    - name: SECURITY_CODE
      description: The code of the security in the position.
      tests:
        - relationships:
            to: ref('REF_ABC_BANK_SECURITY_INFO')
            field: SECURITY_CODE
```

We have added a relationships test to the SECURITY_CODE column of our fact.

The test declares that the column in the fact is a foreign key for a relationship with the security dimension, where the SECURITY_CODE field is the PK.

The test passes, so we can move on comforted by the knowledge that we do not have orphans, even if we know that sooner or later, we will have some orphans and will need to manage them. We will do that when we talk about refactoring our models.

Testing that the relationships that we expect between our models work as expected is extremely important to guarantee that our data models keep being appropriate.

It is also worth noting that, in general, we have one relational data model in the refined layer, between the REF models, that tries to broadly represent the real world and the processes of our domain, but we also have a Kimball (or star schema) model for each data mart that we create. They are related but not equal. Where should we test the relationships?

The golden rule is, as soon as possible! Therefore, we should test the relationships that are part of the REF layer data model in the REF layer, and then we should test the eventual relationship that is created only for the data mart in the data mart layer.

## Testing the delivery layer

Looking at our models in the delivery layer, we can see that they are both a very simple SELECT * FROM aRefModel type, so we will just make sure that the PKs are respected and that the dimension has a default key.

Let's start with the PKs:

1. Create the _portfolio.yml file in the data mart folder.

2. Add the following code to the file:

```
version: 2
models:
  - name: FACT_POSITION
```

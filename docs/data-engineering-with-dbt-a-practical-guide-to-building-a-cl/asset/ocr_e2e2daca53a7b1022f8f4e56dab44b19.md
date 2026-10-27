Second, we will replace the `SNSH_ABC_BANK_SECURITY_INFO` snapshot with the `HIST_ABC_BANK_SECURITY_INFO` table in the `REF_ABC_BANK_SECURITY_INFO` model:

+   1. Create a new file named `HIST_ABC_BANK_SECURITY_INFO` in the staging folder and add the following code:

```
```sql
{{ save_history(
    input_rel = ref('STG_ABC_BANK_SECURITY_INFO'),
    key_column = 'SECURITY_HKEY',
    diff_column = 'SECURITY_HDIFF',
) }}
```
```

Enter the code and click the **Save** button. Then, click the **Build** button or run the `dbt build -s HIST_ABC_BANK_SECURITY_INFO` command.

+   2. Edit the `REF_ABC_BANK_SECURITY_INFO` model so that it uses the HIST table as input in place of the snapshot.

Here is the `current_from_snapshot` CTE:

```
```sql
current_from_snapshot as (
    {{ current_from_snapshot(
        snsh_ref = ref('SNSH_ABC_BANK_SECURITY_INFO'),
    ) }}
)
```
```

Replace it with the `current_from_history` CTE, as shown here:

```
```sql
current_from_history as (
    {{ current_from_history(
        history_rel = ref('HIST_ABC_BANK_SECURITY_INFO'),
        key_column = 'SECURITY_HKEY',
    ) }}
)
```
```

When you are done, save and build the models.

You can find the full REF files with the code to use the HIST models in the *Chapter 14* materials in the repository associated with this book.

We have now created and deployed the two HIST models and refactored the REF models to use the data coming from the HIST models. Now, the lineage graph looks like this:

![](bbox=[0.149, 0.792, 0.862, 0.851])

Figure 14.1: Lineage graph after the last refactoring
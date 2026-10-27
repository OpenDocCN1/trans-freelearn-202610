+   - The definition of the strategy to be used to look for changes – that is, to identify new instances or new versions of a tracked instance:
+     `strategy='check'`,
+   - `check_cols=['POSITION_HDIFF']`,
+   - The configuration to enable tracking of deletions from the source:
+     `invalidate_hard_deletes=True`,
+   - The query that is executed to fetch the data to update the snapshot:
+     `select * from {{ ref ('STG_ABC_BANK_POSITION') }}`

We have used the check strategy, as we do not trust the REPORT_DATE column to work as a good timestamp for the timestamp strategy.

Note that to be able to track deletions, you need to have a reliable full export from the source, as any key that is not in the source is considered deleted.

As we have discussed previously in this section, to work around the impossibility of previewing the query used in a snapshot, we have coded our query in an STG model, configured as ephemeral.

Now that we have created our first snapshot, let’s run it with the following command:

```
dbt snapshot
```

After the run completes in a few seconds, you should find a new table with the name of your snapshot in the database, and the schema that we have configured in the main dbt configuration file, dbt_project.yml.

It is important to remember that snapshots are a global, shared resource, as they end up in the same schema for all the developers and environments that share the same database.

Now that we have the snapshot containing our the data, it’s time to connect the REF model with the snapshot in place of the STG model.

# Connecting the REF layer with the snapshot

The REF_POSITION_ABC_BANK model that we wrote before is still reading data from the STG model that we made ephemeral. It still works, but it’s now time to make it read from the snapshot so that our ELT will keep working even if the external source is not available.

If you just change the name of model inside the ref() function in your REF model, the effect would be to read everything out of the history – that is, all the versions that you encountered in the history of the snapshot. This is not what we want.
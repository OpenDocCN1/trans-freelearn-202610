The suggestion is, as always, to keep it as simple as possible, grouping the resources that have the same frequency and using the selection syntax to write resilient commands, using source systems (or specific sources) and data marts as the high-level logical specification of what to run (independent by specific model names), and working with the graph operators to include what is needed.

This often boils down to building all that is needed for a data mart or everything that depends on a certain data source or a specific set of source tables. Your mileage may vary and there are good reasons to craft other kinds of selections.

Last but not least, remember that you can apply the selection syntax to any dbt command, so you can create jobs that just test specific models, sources, or data marts and use them to notify people of eventual issues.

# Special job – full refreshes and ad hoc commands

Another type of job that we want to introduce is a job that you could see as the equivalent of the command line of the IDE: a job to execute specific one-off commands on a specific environment.

Some reasons to have such a job are as follows:

+   - To maintain alignment between the different environments, we do not want to allow developers to execute manual, unaudited SQL commands on any environment that is not their dev environment. Experience shows that manual commands are the recipe for divergent environments, which, in turn, defeats the purpose of having them. Without full alignment, something that works well in one environment is not guaranteed to work in another.
+   - We need a way to perform maintenance operations. In general, we can perform them using `run-operation`, which we will see in the next section, but in some cases, it is simpler and safer to just run a dbt command.
+   - If you use variables or environment variables to drive your process, you will have default values set in YAML files or in the periodic jobs that run the processes. Sometimes, you might want to do a limited, special run with different parameters without editing the main periodic job. Consider, as an example, re-running only a subset of the graph reading the last month of ingested data instead of the last couple of days, as done in the usual job.
+   - Doing special runs to reset some incremental tables (using the `-full-refresh` option) when you want to purge the old data or apply changes to the schema (also purging the old data). The alternative is to write a migration, which is a set of SQL commands to alter the table or its content, as we will see in the next section.

Having a single job that you use to do all these special, one-off runs makes it very easy to look back to find details of such special runs and gives you the peace of mind that the execution and its logs are stored and well documented, contrary to the alternative of temporarily changing some periodic job.
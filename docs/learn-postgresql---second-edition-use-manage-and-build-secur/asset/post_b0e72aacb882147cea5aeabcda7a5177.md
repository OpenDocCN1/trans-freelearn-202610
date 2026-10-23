
Log rotation is useful because it allows you to produce smaller log files that can be constrained to a specific period of time. On one hand, this is going to scatter the logs across multiple (possibly small) files; on the other hand it will not produce a single (possibly huge) log file that can be problematic to analyze.

Once you have enabled the logging collector, you have to configure it so that it will store the logs as you want and where you want. In fact, you can use the following parameters to configure the logging collector process, by placing the right value for any of the following settings in the PostgreSQL configuration file:

- log_directory: This is a directory where individual log files must be stored. It can be a relative path, considered with regard to PGDATA, or an absolute path (which the process must be able to write into). Clearly, it must be a path where the operating system user running the cluster has write access.
- log_filename: This is a single filename or a pattern to specify the name of every log file (within log_directory). The pattern can be specified following strftime(3) to format it with a date and time. For example, the value postgresql-%Y-%m-%d.log will produce a log filename with the date (respectively, year, month, and day), for example, postgresql-2022-07-19.log.
- log_rotation_age: This indicates how much time the log should wait before applying automatic log rotation. For example, 1d means 1 day and specifies that the logs will be rotated once per day.
- log_rotation_size: This specifies the size of the log file before it is rotated to a new one. For example, 50MB means that the log file will be rotated once it has reached a size of 50 MB.
- log_truncate_on_rotation: This boolean parameter determines whether PostgreSQL must truncate (i.e., empty and start over) an existing file when rotating, or instead append the new log data to the existing file.

Be aware that log rotation is not exact: the log files could slightly exceed the rotation size or age depending on the cluster logging activity.

All the rotation-related settings require the logging_collector to be turned on: after all, PostgreSQL can manage rotation only if it is in charge of the logging.

An example logging configuration within the `postgresql.conf` file could look as follows:

```conf
logging_collector  = on
log_destination    = 'stderr,csvlog,jsonlog'
log_directory      = 'log'
log_filename       = 'postgresql-%Y-%m-%d.log'
log_rotation_age   = '1d'
log_rotation_size  = '50MB'
```

With the preceding settings, the cluster will produce a new log file for each day (or 50 MB of information) within the log directory (relative to PGDATA) using the logging collector, and every log file will have the indication of the year, month, and day it was created. Note that, as an example, the system will produce logs in textual, JSON, and CSV formats simultaneously; PostgreSQL will smartly change the log filename extension to `.json` and `.csv` for the latter two formats respectively.

With the above logging configuration (also used in the chapter’s Docker image), inspecting the log directory will produce an output similar to the following one:

```sh
$ ls -1 /postgres/16/data/log
postgresql-2023-07-19.csv
postgresql-2023-07-19.json
postgresql-2023-07-19.log
...
```

The `.log` file is the one with plain textual format, while the other two files have CSV and JSON format entries.

## When to log

It is important to decide when an event must be reported in the logs. There are a lot of options to control the triggering of a log action, specified by means of a threshold. The logging threshold can assume a mnemonic value that indicates the minimum value over which the log event will be inserted into the logs.

The most common values are, in order, info, notice, warning, error, log, fatal, and panic, with info being the minimum and fatal being the highest value.

As an example, if you decide that warning is the threshold you want to accept as a minimum, every log event with a lower threshold (such as info and notice) will not be inserted into the logs.

As you can see, the threshold increases as it moves toward error values such as fatal and panic, which are always logged automatically because they represent unrecoverable problems. There are also the lowest levels named debug1 through debug5 to get development information and inner details about the process executions (that is, they are usually used when developing with PostgreSQL).

The cluster will therefore produce different log events at different times, and all with different levels of priority, which in turn will be inserted into the logs depending on the threshold you have configured.

In particular, there are two parameters that can be used to tune the log threshold: `log_min_messages` and `client_min_messages`.

The former, `log_min_messages`, decides the threshold of the logging system, while the latter decides the threshold of every new user connection. How are they different?

`log_min_messages` specifies what the cluster has to insert into the logs without any regard for incoming user connections, nor their settings. `client_min_messages` decides which log events the client has to report to the user during the connection. Both these settings can assume a value from the preceding list of thresholds.

A typical use case of a development or test environment could be the following:

```text
log_min_messages    = 'info'
client_min_messages = 'debug1'
```

With the preceding configuration, the cluster will log only info messages in the textual logs, which is something related to the normal execution of the processes, while incoming user connections will report more detailed messages such as development ones back to the user.

Setting thresholds is not the only way you can decide when to trigger log insertion: there are another couple of settings that can be used to take care of the duration of statements and utilities.

If you are interested in logging statements (i.e., queries) executed by your clients, you have the following logging parameters to tune:

- `log_min_duration_statement` holds an integer value that represents a number of milliseconds. Every statement taking more time than the set value will be logged. Therefore, setting this value to 0 means that every statement occurring in the system will be dumped into the logs.
- `log_min_duration_sample` and `log_statement_sample_rate` are parameters that work together. `log_min_duration_sample` handles a number of milliseconds and logs only a sample of statements running for much longer than the value of milliseconds. In other words, it works similarly to `log_min_duration_statement` but instead of logging every statement, it logs only a fraction of them. The fraction of statements to be logged is decided by `log_statement_sample_rate`, which handles a value between 0 and 1.
- `log_transaction_sample_rate` is a value between 0 and 1 that indicates how many transactions will be fully logged (i.e., every statement of the transaction will appear in the logs) regardless of the statement durations.

The idea behind the sample parameters is to reduce the amount of logging activity (and size), still providing a useful insight on what is happening in the cluster.

In order to better understand the above parameters, consider the following configuration:

```
log_min_duration_statement = 500
log_min_duration_sample = 100
log_statement_sample_rate = 0.8
log_transaction_sample_rate = 0.5
```

The above configuration will log every statement that runs for more than 500 milliseconds (`log_min_duration_statement`) and 80% of every statement taking longer than 100 milliseconds (`log_min_duration_sample` and `log_statement_sample_rate`). Finally, it will log every transaction out of two (`log_transaction_sample_rate`).

You can test the above with the following simple workload:

```
forumdb=> BEGIN;
BEGIN
forumdb=> SELECT 'transaction 1';
 ?column?
-------------
 transaction 1
(1 row)
forumdb=> ROLLBACK;
ROLLBACK
forumdb=> BEGIN;
```

```
process-max=2
repo-path=/var/lib/pgbackrest
repo1-retention-full=2
repo1-retention-archive=5
repo1-retention-diff=3
log-level-console=info
log-level-file=info
```

We see the following options here:

+   - start-fast=y: Forces a checkpoint on the remote server, so that pg_start_backup () starts as soon as possible.
+   - archive-async=y: Enables the asynchronous transfer of WAL for push/pull operations.
+   - process-max=2: Sets the maximum number of processes that the system can use for transfer/compression operations.
+   - repo-path=/var/lib/pgbackrest: Sets the path where the repository will be stored; the user running the pgbackrest command must have read/write permissions for this directory.
+   - repo1-retention-full=2: The number of full backups to keep. When a full backup expires, all differential and/or incremental backups associated with the full backup will also expire. When the option is not defined, the system issues a warning. If indefinite retention is desired, set the option to the maximum value (9,999,999).
+   - repo1-retention-archive=5: Represents the backup number of the WAL files to keep. The WAL segments required to make a backup consistent are always maintained until the backup expires, regardless of the configuration of this option. If this value is not set, the expiring archive will automatically expire at the repo-retention-full (or repo-retention-diff) value corresponding to the type of repo-retention archive if set to full (or diff). This will ensure that the WAL files are considered expired only for backups that have already expired.
+   - repo1-retention-diff = 3: The number of differential backups to keep. When a differential backup expires, all incremental backups associated with the differential backup will also expire. If not defined, all differential backups will be kept until the full backups on which they depend expire.
+   - log-level-console=info/log-level-file=info log: Settings for log management; set the terminal log level (log-level-console) and the logging level on the log file (log-level-file).
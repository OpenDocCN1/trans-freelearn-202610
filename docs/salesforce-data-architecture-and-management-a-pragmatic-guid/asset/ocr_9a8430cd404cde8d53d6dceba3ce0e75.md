+   - Group membership locks: There are a few operations that cause an org-wide lock. This means that you cannot use parallel processing in this case because once a lock has been acquired on the group membership tables, no other updates can be made unless the lock is released. Some of these operations are as follows:
+       - Adding users who are assigned to roles
+       - Changing user roles
+       - Adding a role to the role hierarchy or territory to the territory hierarchy
+       - Adding/removing members from the public or personal groups, roles, territories, and queues
+       - Changing the owner of an account to a new owner while this new owner has a different role than the original account record owner. This applies to accounts that have a community role or portal roles associated with them

Ensure that your code executes in such a way that the lock on group membership objects is minimal and other code is not causing the locks to be prolonged. When group membership type operations are executing, the run must be in serial mode due to the org-wide record locking when these operations are run.

+   - Overlapping runs: Multiple running jobs that overlap can cause unintended lock contention on objects, code, and workflow rules that Salesforce runs as part of the job. Ensure that there is sufficient time between jobs so that the previous one has run completely before the subsequent job.

+   - Roll-up summary fields: When records on the detail side are being updated, the master record is also locked to ensure that the roll-up summary is updated. This can cause lock contention and, if possible, these should be removed before loading data. If that is not possible, then consider using summary reporting on the detail object if it has all the data that's required or a report that spans the master and detail objects. If that is not an option, then order your master object records and avoid having the same master record in multiple batches.

As seen, there are many considerations for efficiently using the Bulk API compared to the other APIs and, usually, the complexities associated with the Bulk API are abstracted by third-party applications that leverage the API. Nonetheless, the Bulk API is the API of choice for data migrations and integrations when large volumes of data are under consideration.
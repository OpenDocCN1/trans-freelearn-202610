strategy.

+   - **Data retention policies:** Specify how long backups are kept before they are deleted. This ensures compliance with legal and regulatory requirements and optimizes storage usage. Setting clear retention rules helps manage the lifecycle of backup data systematically, preventing unnecessary storage consumption and maintaining a clean backup environment.
+   - **Automatic pruning of outdated backups:** Reduces storage costs and management overhead, ensuring that only relevant backups are retained. Implementing automatic pruning involves configuring backup tools to delete old backups at regular intervals, thus maintaining an efficient and cost-effective backup repository.
+   - **Incremental backup implementation:** Capture only changes made since the last backup, reducing the backup size and minimizing storage requirements to enhance backup efficiency and decrease the time needed for backups. Configuring backup systems to perform incremental rather than full backups can significantly optimize resource use and improve recovery times.

# DR strategies

+   - **Multi-zone/region availability:** Deploying Kubernetes clusters across multiple zones or regions provides resilience against zone-specific failures. If one zone goes down, the other can continue to operate, minimizing downtime.
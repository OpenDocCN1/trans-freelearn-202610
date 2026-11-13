launches the containers but keeps a watch on the resources usage of the task. If, for example, the container starts utilizing more resources than it has been provisioned then that container is killed by the NM. This greatly improves the job isolation and fair sharing of resources guarantee that YARN provides as, otherwise, it would have impacted the execution of other containers. However, it is important to note that the job status and application status as a whole are managed by the AM. It falls in the domain of the AM to continuously monitor any delay or dead containers, simultaneously negotiating with RM to launch new containers to reassign the task of dead containers.

+   5. The containers executing on different nodes send application-specific statistics to the AM at specific intervals.

+   6. The AM also reports the status of the application directly to the client that submitted the specific application, in our case a Spark job.

+   7. The NM monitors the resources being utilized by all the containers on the respective nodes and keeps sending a periodic update to RM.

+   8. The AM sends periodic statistics such application status, task failure, and log information to RM.
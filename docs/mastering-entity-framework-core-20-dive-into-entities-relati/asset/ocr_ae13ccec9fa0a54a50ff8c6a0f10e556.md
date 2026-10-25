In the first tab, we are retaining the value and performing an update that will update the values, since EF will allow one update to happen irrespective of the values updated in any of the columns (except `Timestamp`, which should not be updated by the user):

![](bbox=[0.23, 0.19, 0.77, 0.7])

The **Post** entity data will be persisted without any issues since the `Timestamp` value doesn't change and it matches the value from the database.
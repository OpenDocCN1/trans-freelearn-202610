This essentially demonstrates the advantage of using the right compute capacity for specific use cases to get better performance.

The preceding exercise also shows how easy it is to read data in different formats and highlights the benefits of storing data in the Parquet file format for analytical use cases.

# Other awswrangler S3 functions

The following are some of the additional functions that awswrangler can perform:

+   Copy objects: We use the S3.copy function from boto3 to copy one object at a time. Typically, users iterate over a function to copy multiple objects. awswrangler makes it easy to simply copy multiple files from one S3 prefix/folder to another S3 location. The copy function appends the source files to the new location. The merge objects function can either overwrite the files in the target location or append files to the target location based on configuration parameters.

+   List objects: This function takes suffix and prefix arguments to filter files from the S3 path. It takes the wildcard character format for file listing. This is achieved through a filename match library (https://docs.python.org/3/library/fnmatch.html). It can also filter files by timestamp after listing the files, which is very advantageous when listing files from a path for incremental data processing.

The following screenshot shows the list of files that are created after a specific timestamp from an S3 location. You can see that the location already has a CSV file, but the list object doesn't list any files when we restrict the list to show only the files that were changed/uploaded in the last minute.

```
```
#This filter is done after listing of files from S3 path
path='s3://athenaresults-sankar/awswrangler/data/'
wr.s3.list_objects(path)
```
```text
: ['s3://athenaresults-sankar/awswrangler/data/9f41c2b19014610a9b972f1635f23a6.csv']
```
```python
#List files created in the last 1 minute
from datetime import datetime,timezone,timedelta
wr.s3.list_objects(path,last_modified_begin=datetime.now(timezone.utc)- timedelta(minutes = 1))
```
```text
: []
```
```

![](bbox=[0.14, 0.73, 0.71, 0.75])

Create a CSV file in the same location and list objects that were created in the last minute. Now, you can see that the new file is listed in the S3 path and does not list the older files. This can help you to identify new files that were created after a specific time period.
```
```java
public static final String SYNC_FILE_KEY = "file",
public static final String SYNC_ENDPOINT_KEY = "http_endpoint";

// Variable used to store a reference to the service
private final JobService jobService;

// Constructor
public SyncTask(JobService jobService) {
    this.jobService = jobService;
}

...
}
```
```

As a starting point, we specified the generic AsyncTask class parameter types, setting JobParameters as the parameter for doInBackground and Result as the type returned from doInBackground and passed to the onPostExecute function.

Later, we create the final constant keys used to pass information in the bundle.

The Result type is also recovered from previous sessions to return an error when something wrong happens during the background execution.

Without going into too much detail, we will implement the doInBackground code responsible for uploading the data to the remote web service:

```
```java
@Override
protected Result<JobParameters> doInBackground(
        JobParameters... params) {

    Result<JobParameters> result = new Result<JobParameters>();
    HttpURLConnection urlConn = null;
    try {
        URL url;
        ...
        // Retrieve the file to upload from the parameters
        // passed in
        String file = params[0].getExtras().
                getString(SYNC_FILE_KEY);

        // Remote WebService Path
        String endpoint = params[0].getExtras().
                getString(SYNC_ENDPOINT_KEY);

        url = new URL ("http://<webs_host>:<webs_port>/"
                + endpoint);

        ...
        // Load the account information stored internally
        String body = Util.loadJSONFromFile(jobService. file);
```
```
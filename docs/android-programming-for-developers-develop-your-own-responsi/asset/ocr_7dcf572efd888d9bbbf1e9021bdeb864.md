If everything goes well, a notification should appear in the notification drawer presenting the success message and the time when the last synchronization happened successfully.

Finally, we can update the SyncJobService code to start and stop the SyncTask execution:

```
public class SyncJobService extends JobService {
    private static final String TAG = "SyncJobService";
    SyncTask mJob = null;
    @Override
    public boolean onStartJob(JobParameters params) {
        Log.i(TAG, "on start job: " + params.getJobId());
        if ( mJob != null ) {
            mJob = new SyncTask(this);
            mJob.execute(params);
            return true;
        }
        return false;
    }

    @Override
    public boolean onStopJob(JobParameters params) {
        Log.i(TAG, "on stop job: " + params.getJobId());
        if ( mJob != null ) {
            mJob.cancel(true);
            mJob = null;
        }
        return true;
    }
}
```
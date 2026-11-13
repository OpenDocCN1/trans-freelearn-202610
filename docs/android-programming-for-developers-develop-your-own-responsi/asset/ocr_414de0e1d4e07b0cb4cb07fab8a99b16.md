# AsyncTask Execution State

The execute() method, could finish in a cancelled state or in a completed state, however if the user tries to call execute() a second time, the task will fail and throw an IllegalStateException exception saying:

> _Cannot execute task, a task can be executed only once/the task is already running_

With a reference to an AsyncTask object in hand, we can ascertain the status of your task over the getStatus() method, and react according to the status result. Let's take a look at the next snippet:

```
// Create a download task object
DownloadImageTask task = new DownloadImageTask(
					ShowMyPuppyActivity.this, iv);
	...
if ( task.getStatus() == AsyncTask.Status.PENDING ) {
	// DownloadImageTask has not started yet so
	// we can invoke execute()
} else if (task.getStatus() == AsyncTask.Status.RUNNING) {
	// DownloadImageTask is currently running in
	// doInBackground()
} else if (task.getStatus() == AsyncTask.Status.FINISHED
	& task.isCancelled()) {
	// DownloadImageTask is done OnCancelled was called
} else {
	// DownloadImageTask is done onPostExecute was called
}
```

Using the getStatus() instance method provided by AsyncTask we can keep up with the execution of the background task and know exactly what the current status of your background work is.

# Note

If you want to repeat your background you have to instantiate a new task and call the execute() method again.
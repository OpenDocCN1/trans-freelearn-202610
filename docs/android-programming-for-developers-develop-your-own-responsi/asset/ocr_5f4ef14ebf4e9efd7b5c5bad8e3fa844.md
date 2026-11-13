happen during the upstream request. This special purpose class could be used in background tasks that don't produce any results:

```
public abstract class AsyncJob
extends AsyncTask<Void, Void, Result<Void>> > {
	@Override
	protected Result<Void> doInBackground(Void ...args) {
	Result<Void> result = new Result<Void>());
	try { runOnBackground() }
	catch (Throwable e) { result.error = e; }
	return result;
	}
	@Override
	protected void onPostExecute(Result<Void> result) {
	if ( result.error != null ) { onFailure(result.error));}
	else { onSuccess(););}
	}
	// Backgroung Execution Task
	abstract void runOnBackground() throws Exception;
	// Error Callback
	abstract void onFailure(Throwable e));
	// Success Function
	abstract void onSuccess();
}
```

With AsyncJob, we declared three abstract methods that any AsyncJob subclass should provide implementations. runOnBackground should implement the background task, onFailure should be used to handle execution exceptions, and the onSuccess callback is invoked to inform the developer that the job has been successfully completed.

Now we are ready to implement the OnClicklistener that will build up an upstream message and dispatch it to our XMPP server in the background:

```
OnClickListener sendListener = new OnClickListener() {
	@Override
	public void onClick(View v) {
		TextView msgText = (TextView) findViewById(R.id.msg);
		final String msgToSend = msgText.getText().toString();
		msgText.setText("");
		
		new AsyncJob() {
			// ...
```
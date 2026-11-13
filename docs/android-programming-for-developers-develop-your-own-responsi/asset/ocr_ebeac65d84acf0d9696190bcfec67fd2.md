# Providing deterministic progress feedback

Knowing that something is happening is a great relief to our users, but they might be getting impatient and wondering how much longer they need to wait. Let's show them how we're getting on by adding a progress bar to our dialog.

Remember that we aren't allowed to update the user interface directly from doInBackground(), because we aren't on the main thread. How, then, can we tell the main thread to make these updates for us?

AsyncTask comes with a handy callback method for this, whose signature we saw at the beginning of the chapter:

```
protected void onProgressUpdate(Progress... values)
```

We can override onProgressUpdate() to update the user interface from the main thread, but when does it get called and where does it get its Progress... values from? The glue between doInBackground() and onProgressUpdate() is another of AsyncTask's methods:

```
protected final void publishProgress(Progress... values)
```

To update the user interface with our progress, we simply publish progress updates from the background thread by invoking publishProgress() from within doInBackground(). Each time we call publishProgress(), the main thread will be scheduled to invoke onProgressUpdate() for us with these progress values.

The modifications to our running example to show a deterministic progress bar are quite simple. Since we have already defined the DownloadImageTask Progress type as Integer, now, we must change the setting progress values in the range 0 (setProgress) to 100 (setMax) and set the style and the bounds of the progress bar. We can do that with the following additions to onPreExecute():

```
@Override
protected void onPreExecute() {
...
// Sets the progress bar style
```
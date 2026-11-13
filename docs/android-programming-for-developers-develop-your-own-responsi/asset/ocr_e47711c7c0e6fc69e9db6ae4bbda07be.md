Although we could override one or more of these five methods, we will not invoke them directly from our own code. These are callback methods, meaning that they will be invoked for us (called back) at the appropriate time throughout the AsyncTask lifecycle.

The key difference between doInBackground() and the other four methods is the thread on which they execute.

Before any background work begins, onPreExecute() will be invoked and will run synchronously to completion on the main thread when we call the execute (Params...) method.

In the onPreExecute() method, we could set up the task or any progress dialog on the UI to indicate to the user that your task has just begun.

Once onPreExecute() completes, doInBackground() will be scheduled and will start work on a background thread.

During the background work, the developer can publish progress updates from doInBackground(), which trigger the main thread to execute onProgressUpdate with the progress values we provide. Internally, the AsyncTask makes use of a Handler bound to the main Thread Looper to publish results on the main Thread as explained in Chapter 2, Performing Work with Looper, Handler and HandlerThread.

By invoking this on the main thread, AsyncTask makes it easy for us to update the user interface to show progress (remember that we can only update the user interface from the main thread).

When the background work completes successfully, doInBackground() may return a result. This result is passed to onPostExecute(), which is invoked for us on the main thread. With the result received on the onPostExecute(), we can update the user interface with the results of our background processing:

# Note

This pattern of passing data from one thread to another is very important, because it allows us to run intensive and long tasks away from the crucial main thread. This construct simplifies the communication in the main thread and
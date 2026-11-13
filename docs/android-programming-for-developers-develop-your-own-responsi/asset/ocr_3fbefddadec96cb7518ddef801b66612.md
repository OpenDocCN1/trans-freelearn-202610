![](bbox=[0.133, 0.105, 0.683, 0.56])

# Figure 3.3 : Indeterministic Progress Dialog

Once the async task is started, the onPreExecute() callback will create an indeterministic progress dialog and display it as shown in Figure 3.3. The non-cancelable dialog will be placed over the UI screen in an opaque layer with the title defined. By indeterministic, we mean that beforehand, we can’t estimate how much longer we have to wait for the task to complete.

Until the download finishes, and the dialog gets dismissed on onPostExecute(), the user is not able to interact with the application and the dialog will remain in the foreground.

# Note
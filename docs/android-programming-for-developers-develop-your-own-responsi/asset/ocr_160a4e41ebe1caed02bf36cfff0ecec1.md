```
...ing body, urlConn.getOutputStream().write(body);
// Write the information to the remote service
uploadJsonToServer(urlConn, body);
// Process Server Response
...
} catch (Exception e) {
result.exc = e;
} finally {
if ( urlConn != null) {
urlConn.disconnect();
}
}
return result;
}
```

Some implementation details are elided here for brevity, but we have implemented the doInBackground function to read the JSON data stored internally on a device file and we uploaded it over an HttpURLConnection. The Android Activity that displays the form saves and syncs the button to the final user. The **save** button, once pressed, stores the account information in the account.json local file when the **save** button is clicked.

The **sync** button, once clicked, will schedule the job for synchronizing the data with our remote HTTP server. When the job criteria defined by us are fulfilled, doInBackground is called to execute the sync procedure in the background.

Now that we have the code to upload the data to our server, let's finish it by processing the response and server errors:

```
try {
...
int resultCode = urlConn.getResponseCode();
if ( resultCode != HttpURLConnection.HTTP_OK ) {
throw new Exception("Failed to sync with server :" + resultCode);
}
result.result = params[0];
...
```

When an exception occurs, such as the server being down or a server internal error happening, the exception is propagated over our Result object to onPostExecute for further processing.
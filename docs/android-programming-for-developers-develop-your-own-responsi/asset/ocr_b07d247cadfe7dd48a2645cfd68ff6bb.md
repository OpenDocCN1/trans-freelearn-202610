# # Applications of AsyncTask

Now that we have seen how to use AsyncTask, we might ask ourselves when we should use it.

Good candidate applications for AsyncTask tend to be relatively short-lived operations (at most, for a second or two), which pertain directly to a specific Fragment or Activity and need to update its user interface.

AsyncTask is ideal for running short, CPU-intensive tasks, such as number crunching or searching for words in large text strings, moving them off the main thread so that it can remain responsive to input and maintain high frame rates.

Blocking I/O operations such as reading and writing text files, or loading images from local files with BitmapFactory are also good use cases for AsyncTask.

Of course, there are use cases for which AsyncTask is not ideally suited. For anything that requires more than a second or two, we should weigh the cost of performing this operation repeatedly if the user rotates the device, or switches between apps or activities, or whatever else may be going on that we cannot control.

Taking these things into account, and the rate at which complexity increases as we try to deal with them (for example, retained headless fragments!), AsyncTask starts to lose its shine for longer operations.

AsyncTask is often used to fetch data from remote web servers, but this can fall foul of the Activity lifecycle issues we looked at earlier. End users may be working with a flaky 3G or HSDPA connection, where network latencies and bandwidth can vary widely, and a complete HTTP request-response cycle can easily span many seconds. This is especially important when we are uploading a significant amount of data, such as an image, as the available bandwidth is often asymmetric.

While we must perform network I/O off the main thread, AsyncTask is not necessarily the ideal option—as we'll see later; there are more appropriate constructs available for offloading this kind of work from the main thread.
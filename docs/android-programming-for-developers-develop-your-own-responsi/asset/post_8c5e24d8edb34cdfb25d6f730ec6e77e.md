
pool of threads, and allow many tasks to be queued if all threads are currently busy.

This is easily achieved by configuring our own instance of ThreadPoolExecutor as a static member of one of our own classes—for example, our Activity class. Here's how we might configure an executor with a pool of four to eight threads and an effectively infinite queue:

```
private static final Queue<Runnable> QUEUE =
        new LinkedBlockingQueue<Runnable>();
public static final Executor MY_EXECUTOR =
        new ThreadPoolExecutor(4, 8, 1, TimeUnit.MINUTES, QUEUE);
```

The parameters to the constructor indicate the core pool size (4), the maximum pool size (8), the time for which idle additional threads may live in the pool before being removed (1), the unit of time (minutes), and the queue to append work when the pool threads are occupied.

Using our own Executor is then as simple as invoking our AsyncTask as follows:

```
task.executeOnExecutor(MY_EXECUTOR, params);
```

## Common AsyncTask issues

As with any powerful programming abstraction, AsyncTask is not entirely free from issues and compromises. In the next sections we are going to list some of the pitfalls that we could face when we want to make use of this construct in our applications.

### Fragmentation issues

In the Controlling the level of concurrency section, we saw how AsyncTask has evolved with new releases of the Android platform, resulting in behavior that varies with the platform of the device running the task, which is a part of the wider issue of fragmentation.

The simple fact is that if we target a broad range of API levels, the execution characteristics of our AsyncTasks—and therefore, the behavior of our apps—can vary considerably on different devices. So what can we do to reduce the likelihood of encountering AsyncTask issues due to fragmentation?

The most obvious approach is to deliberately target devices running at least Honeycomb, by setting a minSdkVersion of 11 in the Android Manifest file. This neatly puts us in the category of devices, which, by default, execute AsyncTasks serially, and therefore, much more predictably.

At the time of writing in October 2015, only 4% of Android devices run a version of Android in the danger zone between API Levels 4 and 10, and therefore targeting your application to Level 11 would not reduce your market reach significantly.

When the ThreadPoolExecutor is used as the executor, the changes introduced in Lollipop (API Level 21) could also bring behavior drifts in relation to older versions (API Level >10). The modern AsyncTask’s ThreadPoolExecutor is limited to the device’s CPU cores * 2 + 1 concurrent threads, with an additional queue of 128 tasks to queue up work.

A second option is to design our code carefully and test exhaustively on a range of devices—always commendable practices of course, but as we’ve seen, concurrent programming is hard enough without the added complexity of fragmentation, and invariably, subtle bugs will remain.

A third solution that has been suggested by the Android development community is to reimplement AsyncTask in a package within your own project, then extend your own AsyncTask class instead of the SDK version. In this way, you are no longer at the mercy of the user’s device platform, and can regain control of your AsyncTasks. Since the source code for AsyncTask is readily available, this is not difficult to do.

### Memory leaks

In cases where we keep a reference to an Activity or a View, we could prevent an entire tree of objects from being garbage collected when the activity is destroyed. The developer needs to make sure that it cancels the task and removes the reference to the destroyed activity or view.

### Activity lifecycle issues

Having deliberately moved any long-running tasks off the main thread, we've made our applications nice and responsive—the main thread is free to respond very quickly to any user interaction.

Unfortunately, we have also created a potential problem for ourselves, because the main thread is able to finish the Activity before our background tasks complete. The Activity might finish for many reasons, including configuration changes caused by the user rotating the device (the Activity is destroyed and created again with a new address in the memory), the user connecting the device to a docking station, or any other kind of context change.

If we continue processing a background task after the Activity has finished, we are probably doing unnecessary work, and therefore wasting CPU and other resources (including battery life), which could be put to better use.

On occasions after a device rotation, the AsyncTask continues to be meaningful and has valid content to deliver, however, it has reference to an activity or a view that was destroyed and therefore is no longer able to update the UI and finish its work and deliver its result.

Also, any object references held by the AsyncTask will not be eligible for garbage collection until the task explicitly nulls those references or completes and is itself eligible for GC (garbage collection). Since our AsyncTask probably references the Activity or parts of the View hierarchy, we can easily leak a significant amount of memory in this way.

A common usage of AsyncTask is to declare it as an anonymous inner class of the host Activity, which creates an implicit reference to the Activity and an even bigger memory leak.

There are two approaches for preventing these resource wastage problems.

### Handling lifecycle issues with early cancellation

First and foremost, we can synchronize our AsyncTask lifecycle with that of the Activity by canceling running tasks when our Activity is finishing.

When an Activity finishes, its lifecycle callback methods are invoked on the main thread. We can check to see why the lifecycle method is being called, and if the Activity is finishing, cancel the background tasks. The most appropriate Activity lifecycle method for this is onPause, which is guaranteed to be called before the Activity finishes:

```
protected void onPause() {
    super.onPause();
    if ((task != null) && (isFinishing()))
        task.cancel(false);
}
```

If the Activity is not finishing—say, because it has started another Activity and is still on the back stack—we might simply allow our background task to continue to completion.

This solution is straightforward and clean but far from ideal because you might waste precious resources by starting over the background work again unaware that you might already have a valid result or that your AsyncTask is still running.

Beyond that, when you start multiple AsyncTasks and start them again when the device rotation happens, the waste grows substantially since we have to cancel and fire up the same number of tasks again.

### Handling lifecycle issues with retained headless fragments

If the Activity is finishing because of a configuration change, it may still be useful to use the results of the background task and display them in the restarted Activity. One pattern for achieving this is through the use of retained Fragments.

Fragments were introduced to Android at API level 11, but are available through a support library to applications targeting earlier API Levels. All of the downloadable examples use the support library, and target API Levels 7 through 23. To use Fragment, our Activity must extend the FragmentActivity class.

The Fragment lifecycle is closely bound to that of the host Activity, and a fragment will normally be disposed when the activity restarts. However, we can explicitly prevent this by invoking setRetainInstance(true) on our Fragment so that it survives across Activity restarts.

Typically, a Fragment will be responsible for creating and managing at least a portion of the user interface of an Activity, but this is not mandatory. A Fragment that does not manage a view of its own is known as a headless Fragment. Since they do not have a UI related to them, they do not have to be destroyed and recreated again when the user rotates the device, for example.

Isolating our AsyncTask in a retained headless Fragment makes it less likely that we will accidentally leak references to objects such as the View hierarchy, because the AsyncTask will no longer directly interact with the user interface. To demonstrate this, we'll start by defining an interface that our Activity will implement:

```
public interface AsyncListener {
    void onPreExecute();
    void onProgressUpdate(Integer... progress);
    void onPostExecute(Bitmap result);
    void onCancelled(Bitmap result);
}
```

Next, we'll create a retained headless Fragment, which wraps our AsyncTask. For brevity, doInBackground is omitted, as it is unchanged from the previous examples—see the downloadable samples for the complete code.

```
public class DownloadImageHeadlessFragment extends Fragment {
    // Reference to the activity that receives the
    // async task callbacks
    private AsyncListener listener;
    private DownloadImageTask task;

    // Function to create new instances
    public static DownloadImageHeadlessFragment newInstance(String url) {
        DownloadImageHeadlessFragment myFragment = new DownloadImageHeadlessFragment();
        Bundle args = new Bundle();
        args.putString("url", url);
        myFragment.setArguments(args);
        return myFragment;
    }

    // Called to do initial creation of fragment
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setRetainInstance(true);
    }
}

# Fragmentation issues

In the Controlling the level of concurrency section, we saw how AsyncTask has evolved with new releases of the Android platform, resulting in behavior that varies with the platform of the device running the task, which is a part of the wider issue of fragmentation.

The simple fact is that if we target a broad range of API levels, the execution characteristics of our AsyncTasks—and therefore, the behavior of our apps— can vary considerably on different devices. So what can we do to reduce the likelihood of encountering AsyncTask issues due to fragmentation?

The most obvious approach is to deliberately target devices running at least Honeycomb, by setting a minsdkVersion of 11 in the Android Manifest file. This neatly puts us in the category of devices, which, by default, execute AsyncTasks serially, and therefore, much more predictably.

At the time of writing in October 2015, only 4% of Android devices run a version of Android in the danger zone between API Levels 4 and 10, and therefore targeting your application to Level 11 would not reduce your market reach significantly.

When the ThreadPoolExecutor is used as the executor, the changes introduced in Lollipop (API Level 21) could also bring behavior drifts in relation to older versions (API Level >10). The modern AsyncTask’s ThreadPoolExecutor is limited to the device’s CPU cores * 2 + 1 concurrent threads, with an additional queue of 128 tasks to queue up work.

A second option is to design our code carefully and test exhaustively on a range of devices—always commendable practices of course, but as we’ve seen, concurrent programming is hard enough without the added complexity of fragmentation, and invariably, subtle bugs will remain.

A third solution that has been suggested by the Android development community is to reimplement AsyncTask in a package within your own project, then extend your own AsyncTask class instead of the SDK version. In this way, you are no longer at the mercy of the user’s device platform, and can regain control of your AsyncTasks. Since the source code for AsyncTask is readily
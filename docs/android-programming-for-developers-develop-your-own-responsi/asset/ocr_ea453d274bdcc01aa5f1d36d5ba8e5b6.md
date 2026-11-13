# # Android concurrency constructs

The good news is that the Android platform provides specific constructs to address the concurrency general issues and to solve the specific problems presented by Android.

There are constructs that allow us to defer tasks to run later on the main thread, communicate easily between cooperating threads, and issue work to the managed pools of worker threads and reintegrate the results back in the main thread.

There are solutions to the constraints of the Activity lifecycle, both for medium-term operations that closely involve the user interface and longer-term work that must be completed even if the user leaves the application.

While some of these constructs were only introduced with newer releases of the Android platform, they are available through the support libraries, and with a few exceptions, the examples in this book target devices that run API level 8 (Android 2.2) and higher versions.
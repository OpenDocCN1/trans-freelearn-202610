Ideally, of course, we don't want to drop a single frame. Jank, unresponsiveness, and especially the ANR, offer a very poor user experience, which translates into bad reviews and unpopular applications. A rule to live by when building Android applications is: do not block the main thread!

# # Note

Android provides a helpful strict mode setting in Developer Options on each device, which will flash on the screen when applications perform long-running operations on the main thread.

Further protection was added to the platform in Honeycomb (API level 11) with the introduction of a new Exception class, NetworkOnMainThreadException, a subclass of RuntimeException that is thrown if the system detects network activity initiated on the main thread.
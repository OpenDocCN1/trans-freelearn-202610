![](bbox=[0.118, 0.091, 0.882, 0.362])

Beyond the ability to dispatch downstream messages from your server (using HTTP or XMPP protocol messages) to your Android application, the GCM framework provides a battery-efficient communication channel to dispatch upstream messages from your application to a XMPP server managed by you.

The GCM client that runs on the Android device provides a reliable and battery-efficient connection between your GCM server and the device. The connection maintained is highly optimized to minimize bandwidth and battery consumption. Therefore, the use of GCM for applications that require high-frequency network data updates, such as real-time messaging, is extremely recommended.

Beyond that, when the device is offline and is not able to contact the GCM service, the platform is able to retain the messages in queues until a maximum number of 20 queued messages, and ensure the delivery of the messages as soon as the device goes online again.
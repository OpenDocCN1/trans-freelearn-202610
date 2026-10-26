```
event Action Suspending;

event Action Resuming;
}
}
```

Just two events with the simplest type of event handler: an Action with no arguments and no return value.

# The iOS implementation

The constructor of the iOS implementation of this interface sets handlers for the iOS ObserveDidEnterBackground and ObserveWillEnterForeground notifications. When these notifications occur, the handlers fire Suspending and Resuming events, respectively:

```
[assembly: Dependency(typeof(PlatformHelpers.iOS.LifecycleHelper))]

namespace PlatformHelpers.iOS
{
    class LifecycleHelper : ILifecycleHelper
    {
        public event Action Suspending;

        public event Action Resuming;

        public LifecycleHelper()
        {
            UIApplication.Notifications.ObserveDidEnterBackground(
                (sender, args) =>
                {
                    if (Suspending != null)
                        Suspending();
                });

            UIApplication.Notifications.ObserveWillEnterForeground(
                (sender, args) =>
                {
                    if (Resuming != null)
                        Resuming();
                });
        }
    }
}
```

Don't forget the Dependency attribute!

# The Android implementation

The Android implementation is a little messier. The Android Activity class defines virtual protected OnPause and OnResume methods, so you'll need to override these methods in the MainActivity class created as part of the standard Xamarin.Forms solution template. These overrides should then call
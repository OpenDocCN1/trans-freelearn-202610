# # Use with sharing Apex classes

Apex code, by default, runs in system mode, which means it has access to _everything_: it can access all objects and their fields, and field-level security and sharing rules don't apply. Check to make sure that your classes are using the `with sharing` keyword because that respects field-level security and sharing rules. The net result of this is that smaller result sets are returned, which can help improve performance.

# ## Devices and browsers

As discussed earlier in this chapter, the Octane score is a good measure of the browser's performance. The higher the score, the better the performance is expected to be. Since the Octane score takes into consideration the user's device as well, it is important to confirm that the device supports the minimum requirements required for the smooth operation of Salesforce. If the user's device is running at the minimum recommended hardware specifications, upgrade the device with additional memory and processing power.

Another thing is to confirm that the device is charged or is not being throttled in some way. Devices usually switch to low-power mode when they are close to running out of power or users may have inadvertently changed the settings, causing the device to operate in a low-powered mode.

If you determined in your initial investigation that the issue is specific to a user, check their browser. Currently, Chrome is the most optimal browser for running Salesforce Lightning. I highly recommend not using Internet Explorer with Salesforce LEX as your users will run into many problems. If the browser is Chrome, then check for its version and whether the version is supported by Salesforce.

Disabling any extensions or plugins in the browser can also help with performance significantly as they actively consume memory and processing resources. If nothing works, then restarting the browser can also help with performance-related issues as resources that are tied up by previous processes are freed.

# ## Visualforce pages

You should have standards on where most development is going to happen, be it Visualforce or Lightning. But considering that Visualforce is the older UI and a lot of custom development has been done in Visualforce, I suggest revisiting the previous release notes and determining whether the custom functionality provided by the Visualforce page is now available through out-of-the-box functionality or by using declarative tools.
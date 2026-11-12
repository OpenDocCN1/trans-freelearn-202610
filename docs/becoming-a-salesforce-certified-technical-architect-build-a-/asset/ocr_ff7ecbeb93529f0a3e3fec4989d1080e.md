Now, take a look at the sequence diagram:

![](bbox=[0.13, 0.14, 0.85, 0.67])

# Figure 4.16 – OAuth 2.0 device flow

This is how the process flows:

+   1. The user turns on the device (for example, turns on the smart TV).
+   2. The device checks for a valid access token or a refresh token. Assume that both are not available because this is the first time the user uses this device.
+   3. The device posts an authorization code request to the authorization server endpoint.
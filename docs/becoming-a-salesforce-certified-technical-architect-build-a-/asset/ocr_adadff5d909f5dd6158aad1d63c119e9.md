![](bbox=[0.13, 0.105, 0.855, 0.555])

# Figure 4.17 – OAuth 2.0 Asset token flow

This is how the process flows:

+   1. The logged-in user attempts to register a new device. Using a specific portal (which is referred to as the client app), the user enters the device ID and clicks a link to start the authentication and registration process.
+   
+   2. The client app generates a JWT, signs it using the app’s private key/certificate, and uses it to request an access token from the authorization server.
+   
+   3. The authorization server receives the token, validates it, and issues an access token (Steps 2 and 3 are meant to request an access token from the authorization server and can be accomplished using either the JWT or web server flow).
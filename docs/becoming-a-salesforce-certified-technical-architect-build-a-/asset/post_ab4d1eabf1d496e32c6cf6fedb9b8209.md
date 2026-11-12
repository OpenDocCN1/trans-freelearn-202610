
Now, take a look at the sequence diagram:

![](img/a3b1eecf3c87c814267f2877e8a8be1a_165_0.png)

## Figure 4.16 – OAuth 2.0 device flow

This is how the process flows:

- 1. The user turns on the device (for example, turns on the smart TV).
- 2. The device checks for a valid access token or a refresh token. Assume that both are not available because this is the first time the user uses this device.
- 3. The device posts an authorization code request to the authorization server endpoint.
- 4. The authorization server verifies the request and issues a human-readable user code/authorization code, normally with a verification URL and a device code. These values are normally displayed on the device’s screen. You would normally see a message on your smart TV screen with the user code, device code, and the URL that the user should use to verify the device (sometimes this is presented as a QR code to make it easier to open the URL using a mobile phone).
- 5. The user is then expected to use a different device (such as a laptop or mobile phone) to open the URL (as it is usually easier considering the limited ability to use a keyboard on a smart TV). The URL would lead the user to a page where they can input the device code and the user code. Once this step is done, the device will be recognized by the authorization server. On some occasions, the user will also be requested to log in using a social network provider (such as Google) and authorize the application. This will look very similar to the web server flow; the user will have to authorize the device to access specific scopes on the resource server.
- 6. The collected data will be used to authenticate and authorize the device. The browser then posts this information to the authorization server.
- 7. The access token and, optionally, the refresh token will be generated.
- 8. During this period, the device will periodically pull the authorization server for the tokens. Once they are available, they are sent to the device.
- 9. Access and refresh tokens are sent to the device and stored securely.
- 10. The device uses the access token to request the desired resource from the resource server.
- 11. The resource server responds by sending the requested resource.

## Asset Token Flow

This flow is mainly created for IoT devices. This could be a bit confusing as smart TVs and appliances could also be considered IoT devices. However, this flow serves a different purpose. It is an open standard meant to verify and secure communication with connected devices, particularly those that do not have any sort of screens to use. In other words, the device would not be able to display a human-readable code to use.

This is common nowadays for many IoT devices such as smart sensors and smart house devices. The user for such devices normally needs to register them before they can be used. Once a device is registered with an IoT management platform, it can be managed and controlled remotely. In addition, its data will start flowing securely to the IoT aggregation platform. The asset token (based on JWT) will be used to uniquely identify and authenticate the specific device (also known as an _IoT asset_). Take a look at the sequence diagram:

![](img/a3b1eecf3c87c814267f2877e8a8be1a_167_0.png)

## Figure 4.17 – OAuth 2.0 Asset token flow

This is how the process flows:

- 1. The logged-in user attempts to register a new device. Using a specific portal (which is referred to as the client app), the user enters the device ID and clicks a link to start the authentication and registration process.
- 2. The client app generates a JWT, signs it using the app’s private key/certificate, and uses it to request an access token from the authorization server.
- 3. The authorization server receives the token, validates it, and issues an access token (Steps 2 and 3 are meant to request an access token from the authorization server and can be accomplished using either the JWT or web server flow).
- 4. The access token is returned to the client app.
- 5. The client app invokes a specific API from the device to request device information. The device is assumed to be online and connected to the internet.
- 6. The device responds by sending device information, such as the device ID, serial number, and others.
- 7. The client app receives the full device information. It can now request an asset token using the access token and, optionally, an actor token. The actor token can be used to pass information (metadata) about the asset/device itself to the authorization server, which could include custom attributes known by the client app (for example, the account ID that this particular asset should be linked with). The actor token itself is in JWT format and can be signed to allow the authorization server to validate it when received. Both the access token and the actor token are posted to the authorization server to exchange them for an asset token.
- 8. The authorization server receives the tokens, validates them, and issues an asset token (in JWT format) signed using its private key/certificate. The asset token can include the standard and custom attributes that describe the asset itself, originally passed in the actor token.
- 9. The asset token is returned to the device. The device can use it from there onward to authenticate to the resource server (the IoT aggregation platform) and send its data.

## Username-Password Flow

This flow utilizes the user credentials (username/password) to retrieve an access token. This flow does not return a refresh token and is meant to be used for specific use cases, mainly when the client app has been developed by the same authority as the authorization server. A high trust level can be assumed in this regard.

For example, you can imagine a website called mywebsite.com trying to access resources from its own subdomain, api.mywebsite.com. However, you are encouraged to consider an alternative flow if possible.

Take a look at its sequence diagram:

![](img/a3b1eecf3c87c814267f2877e8a8be1a_169_0.png)

## Figure 4.18 – Username-password flow

This is how the process flows:

- 1. The user authenticates to the client app using credentials, assuming this is the first time the user has attempted to access the client app today and no session token is available.
- 2. The client app uses the received credentials to request an access token from the authorization server. The app will pass the following parameters: `grant_type` (the value for this flow should be `password`); `client_id` (the client app’s unique ID); `client_secret` (the client secret of the app), `username` (the username captured from the user); and `password` (the password captured from the user). It is worth mentioning that when using this flow to authenticate to a Salesforce API, you need to concatenate the Salesforce security token to the password.
- 3. The authorization server validates the credentials and issues an access token.
- 4. The access token is returned to the client app.
- 5. The client app uses the access token to request the desired resource from the resource server.
- 6. The resource server responds by sending the requested resource.

That concludes the key authentication flows that need to be covered in this chapter. This is by no means a complete list. However, this will get you up to speed with the most common standards and flows you may encounter in your day-to-day work.

## Summary

The chapter started by covering some general concepts regarding IAM. You learned the importance of crafting a well-designed IAM strategy and how that could significantly impact the end user experience as well as overall system security and compliance. You then became familiar with some key IAM terms and definitions, including _identity_, _authentication_, _authorization_, _identity store_, and others. You then moved on to discover some of the most common IAM standards, including SAML, OAuth 2.0, OIDC, and Kerberos, along with the different types of tokens they generate or use, such as access tokens, refresh tokens, session tokens, and ID tokens.

That all set the scene to dive deeper into some of the common and standard authentication flows. You read an in-depth review of nine different flows, including SAML IDP-initiated, SAML SP-initiated, OAuth web server, OAuth JWT, and others.

That concludes this part of the book, where you discussed several common architectural concepts, including data, integration, security and cryptography, and IAM. In the next part, you will dive into more Salesforce-specific knowledge areas and go through some hands-on exercises that will help you to prepare for the CTA review board.

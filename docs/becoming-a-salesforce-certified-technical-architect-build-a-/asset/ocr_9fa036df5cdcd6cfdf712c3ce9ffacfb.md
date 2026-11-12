4. The access token is returned to the client app.

5. The client app invokes a specific API from the device to request device information. The device is assumed to be online and connected to the internet.

6. The device responds by sending device information, such as the device ID, serial number, and others.

7. The client app receives the full device information. It can now request an asset token using the access token and, optionally, an actor token. The actor token can be used to pass information (metadata) about the asset/device itself to the authorization server, which could include custom attributes known by the client app (for example, the account ID that this particular asset should be linked with). The actor token itself is in JWT format and can be signed to allow the authorization server to validate it when received. Both the access token and the actor token are posted to the authorization server to exchange them for an asset token.

8. The authorization server receives the tokens, validates them, and issues an asset token (in JWT format) signed using its private key/certificate. The asset token can include the standard and custom attributes that describe the asset itself, originally passed in the actor token.

9. The asset token is returned to the device. The device can use it from there onward to authenticate to the resource server (the IoT aggregation platform) and send its data.

# Username-Password Flow

This flow utilizes the user credentials (username/password) to retrieve an access token. This flow does not return a refresh token and is meant to be used for specific use cases, mainly when the client app has been developed by the same authority as the authorization server. A high trust level can be assumed in this regard.

For example, you can imagine a website called mywebsite.com trying to access resources from its own subdomain, api.mywebsite.com. However, you are encouraged to consider an alternative flow if possible.
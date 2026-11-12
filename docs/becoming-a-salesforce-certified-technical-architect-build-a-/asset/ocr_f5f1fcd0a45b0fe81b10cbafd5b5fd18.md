6. The user is presented with a login screen served by the authorization server. The user enters their username and password (or authenticates in whatever way defined by the authorization server). If the server is configured to use 2FA or MFA, the other factors will be requested before the user is authenticated. Once that is complete, the user will be presented with a screen asking for approval to allow the client app to access specific scopes on the resource server. The screen will look like the following screenshot:

![](bbox=[0.34, 0.225, 0.625, 0.565])

Figure 4.11 – Example of an OAuth 2.0/OIDC authorization approval screen

7. The user is authenticated, and an authorization code is generated and returned to the client app via the user browser (redirect). The authorization code will be returned to the given redirect_uri. For this example, the returned auth code could look like this: https://packt.link/5i3bi.

8. The authorization code is sent to the client app via the browser. Please note that due to the redirect via the browser, the authorization code might be exposed to attackers. However, it has a very short TTL, and there are other protection mechanisms that prevent attackers from making use of it, such as the need for a client secret value (which is supposed to be known only to the client app).

9. The client app receives the authorization code. It uses it to get an access token from the authorization server. This is achieved by posting the authorization code and other values to a specific server endpoint.
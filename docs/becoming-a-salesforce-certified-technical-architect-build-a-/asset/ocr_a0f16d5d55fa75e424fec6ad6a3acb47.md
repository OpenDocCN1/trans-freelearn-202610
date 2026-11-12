+   2. The website checks for an existing and valid access token or refresh token for the current user and the target resource server. These tokens would typically be stored on the server hosting the website, either in memory, or in a more persistent data store such as a database, or both, identified by the user who owns them and the target system they are meant to be used with. The tokens should be stored safely on the server where an attacker or other users (even a system administrator) do not have access (for example, they could be encrypted while stored in the database and protected while in memory).
+   
+   Assume that the application did not find any tokens for the current user, which are meant for the target resource server, most likely because this is the first time that this user tries to retrieve data from the resource server.

+   3. The client app starts the authorization process by sending an authorization request. This process is sometimes called *the OAuth dance*. The client app does an HTTP redirect via the browser to the target *authorization endpoint*, such as `https://pact.link/2FEvu`, and passes a set of parameters.
+   
+   The key parameters passed are `client_id`, which is the unique alphanumeric ID for the client app, and `redirect_uri`, which is the URI that the user will be redirected to following successful authentication. This URI must also be registered with the authorization server (this is also done during the setup process). The `response_type` parameter defines the OAuth 2.0 grant type; this pretty much defines the type of authentication flow used. For the *web server* flow, the value of this parameter must be `code`.
+   
+   There are several optional parameters that you can use to configure the behavior of the flow. One of them is *scope*. This parameter contains a list of the permissions the application requests to be authorized to use upon successful authentication. The authorization server normally defines the possible values of this parameter.
+   
+   However, there are some common scopes, such as `full` (allows access to all data accessible by the logged-in user), `OpenID` (requesting this scope while using either the user-agent flow or the web server flow enables the app to receive a signed ID token), and `refresh_token` (enables the app to receive a refresh token under specific conditions). For example, the full URI of the redirect could look like `https://pact.link/d2KaB`.

+   4. The authorization request is sent to the authorization server.

+   5. The authorization server checks for an existing and valid session token stored in a cookie associated with the server's domain. Assume that this token has not been found because this is the first time the user has logged in to the authorization server today.
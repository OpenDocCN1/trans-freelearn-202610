
## SAML IDP-Initiated Flow

The following diagram represents a SAML IDP-initiated flow. This flow is usually used by enterprises that provide a central location/page for their employees to access all applications (SPs) linked to the same enterprise IDP:

![](img/a3b1eecf3c87c814267f2877e8a8be1a_150_0.png)

Figure 4.6 – SAML IDP-initiated flow

This is how the process flows:

- 1. The user visits the IDP login page or the IDP home page.
- 2. The home or login pages will check whether the user is authenticated by checking whether there is a stored cookie for the IDP domain with a valid session token. Assume that this is the first login by the user for the given day and that there is no valid session token found. The user should see the login page at this stage.
- 3. The user enters their username and password (or authenticates in the process defined by the IDP). If the IDP is configured to use 2FA or MFA, then the other factors will be requested before the user is authenticated. Once the user is authenticated, they are redirected to the IDP's home page.
- 4. The user lands on the IDP's home page/landing page. The page contains links for all available applications (SPs) that are using the IDP for SSO. The user clicks on one of those links.
- 5. The IDP generates a SAML assertion, signed with the IDP's private key/certificate. The assertion is sent via the browser to the SP.
- 6. The assertion is sent to the SP.
- 7. The SP receives the assertion and validates it using the IDP’s public key/certificate. The user is authenticated, and a session token for the specific SP domain is created and saved in a browser cookie. The user is redirected to the SP home page. The next time the user attempts to access the SP, the SP will detect the cookie with the session token. And if it is still valid, the user will be allowed to access the desired page. Otherwise, a new authentication flow will start over. The only difference is that this time it will start from the SP rather than the IDP. This is what is referred to as SP-initiated flow.

## SAML SP-Initiated Flow

The following diagram represents a SAML SP-initiated flow. This flow is used when a user attempts to access a deep link of a particular SP. The deep link could be a URL saved in bookmarks, a link in an email, a URL from the browser history, a URL that the user knows by heart, or even the URL of the SP’s home page. The idea is that the user is trying to access one of the SP’s resources. The SP needs to authenticate the user before it allows access to that resource. This is a very common flow:

![](img/a3b1eecf3c87c814267f2877e8a8be1a_151_0.png)

Figure 4.7 – SAML SP-initiated flow

This is how the process flows:

- 1. The user visits the SP using a deep link or simply by trying to access the SP’s home page.
- 2. The SP checks for an existing and valid session token stored in a cookie associated with the SP’s domain. Assume that this token has not been found because this is the first time the user attempts to access this application/SP today.
- 3. The user is redirected to the IDP via the browser with a SAML assertion request. The idea here is to get the IDP to authenticate the user and then return the user back to the SP’s page/URL that the user was attempting to access (for example, the URL in a deep link such as http://www.packtpub.com/books/SF_ExamplePage.html). The target URL is passed in a parameter called RelayState. In this example, the value of this parameter would be /books/SF_ExamplePage.html. This parameter will eventually be passed back to the SP following user authentication, which will help redirect the user to the right URL.
- 4. The user is redirected to the IDP login page (with the RelayState value).
- 5. The IDP checks for an existing and valid session token stored in a cookie associated with the IDP’s domain. Assume that this token has not been found because this is the first time the user has logged in to the IDP today.
- 6. The user enters the username and password (or authenticates in whatever way is defined by the IDP). If the IDP is configured to use 2FA or MFA, then the other factors will be requested before the user is authenticated.
- 7. Once the user is authenticated, a SAML assertion will be created, signed with the IDP private key/certificate, and returned to the SP via the browser, along with the RelayState value.
- 8. A SAML assertion is sent to the SP via the browser, along with the RelayState value.
- 9. The SP receives the assertion and validates it using the IDP’s public key/certificate. The user is authenticated, and a session token for the specific SP domain is created and saved in a browser cookie. The user is then redirected to the SP page specified in the RelayState value. The user lands on the URL they were trying to access in Step 1.

This is how it looks the next time the user attempts to access another page from the same SP, assuming the SP session cookie is still valid:

![](img/a3b1eecf3c87c814267f2877e8a8be1a_152_0.png)

Figure 4.8 – Continuing the SAML SP-initiated flow following user authentication with a valid SP session

This is how the process flows:

- 1. The user visits the SP using a deep link or simply by trying to access the SP’s home page.
- 2. The SP checks for an existing and valid session token stored in a cookie associated with the SP’s domain. Assume that the token from the previous authentication is still valid.
- 3. The user is granted access to the desired resource. Now look at the scenario where the user attempts to access a page from the SP following the expiry of the SP’s session token, but prior to the expiry of the IDP’s session token. This could happen if the expiry time of the IDP token is set to a higher duration than the session token of the SP:

![](img/a3b1eecf3c87c814267f2877e8a8be1a_153_0.png)

Figure 4.9 – Continuing the SAML SP-initiated flow with an expired SP session and a valid IDP session

This is how the process flows:

- 1. The user visits the SP using a deep link or simply by trying to access the SP’s home page.
- 2. The SP checks for an existing and valid session token stored in a cookie associated with the SP’s domain. Assume that the token from the previous authentication has expired.
- 3. The user is redirected to the IDP via the browser with a SAML assertion request. The value of RelayState is also populated and passed.
- 4. The user is redirected to the IDP login page (with the RelayState value).
- 5. The IDP checks for an existing and valid session token stored in a cookie associated with the IDP's domain. Assume that the IDP found an active/valid session token.
- 6. A SAML assertion is created, signed with the IDP private key/certificate, and returned to the SP via the browser, along with the RelayState value.
- 7. A SAML assertion is sent to the SP via the browser, along with the RelayState value.
- 8. The SP receives the assertion and validates it using the IDP's public key/certificate. The user is authenticated, and a session token for the specific SP domain is created and saved in a browser cookie. The user is then redirected to the SP page specified in the RelayState value.

These are the most common SAML 2.0 flows. Now, you will explore and learn about the OAuth 2.0/OIDC flows.

## OAuth 2.0/OIDC Flows

Remember that OIDC is based on OAuth 2.0, so most of the following flows are applicable to both. Here are some of the terms used in these flows:

- **Client app**: An application that is trying to access resources on a resource server on behalf of the user. For example, a website such as www.mywebsite.com could be trying to access resources from a Salesforce instance such as MySInstance.my.Salesforce.com. Salesforce Workbench https://pact.link/0oXKc is another good example of a website that is accessing resources from another (a Salesforce org in this case).
- **Authorization server**: This is the server that will grant the right authorization to the logged-in user. For the sake of simplicity, assume that this server is also handling the process of identifying the user attempting to log in (effectively authenticating the user and acting like an IDP) and that it has access to the identity store where the identification mechanisms for the users are kept (such as user credentials). login.Salesforce.com is an example of such a server.
- **Resource server**: This is the server with the resource that the client application is trying to access on behalf of the user. For example, this could be a Salesforce instance such as MySInstance.my.Salesforce.com, from where the user is simply trying to access a particular page or a web service.
- **Client ID/consumer ID**: This is a unique alphanumeric ID for a given client app. The authorization server generates this value for a specific client app upon registering the app with the authorization server. The app registration is typically done as part of the setup process, but it can happen dynamically in some cases.
- **Client secret/consumer secret**: This is a unique alphanumeric value representing a highly confidential value that the client app should never expose to the user or external entities apart from the trusted authorization server. Similar to the client ID, the authorization server also generates this value for a specific client app upon registering the app with the authorization server.

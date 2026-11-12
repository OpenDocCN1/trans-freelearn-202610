This is how the process flows:

+   1. The user visits the SP using a deep link or simply by trying to access the SP’s home page.
+   2. The SP checks for an existing and valid session token stored in a cookie associated with the SP’s domain. Assume that the token from the previous authentication is still valid.
+   3. The user is granted access to the desired resource. Now look at the scenario where the user attempts to access a page from the SP following the expiry of the SP’s session token, but prior to the expiry of the IDP’s session token. This could happen if the expiry time of the IDP token is set to a higher duration than the session token of the SP:

![](bbox=[0.13, 0.29, 0.85, 0.63])

Figure 4.9 – Continuing the SAML SP-initiated flow with an expired SP session and a valid IDP session

This is how the process flows:

+   1. The user visits the SP using a deep link or simply by trying to access the SP’s home page.
+   2. The SP checks for an existing and valid session token stored in a cookie associated with the SP’s domain. Assume that the token from the previous authentication has expired.
+   3. The user is redirected to the IDP via the browser with a SAML assertion request. The value of RelayState is also populated and passed.
+   4. The user is redirected to the IDP login page (with the RelayState value).
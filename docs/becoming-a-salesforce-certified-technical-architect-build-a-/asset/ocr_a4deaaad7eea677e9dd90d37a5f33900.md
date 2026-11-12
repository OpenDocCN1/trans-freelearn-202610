+   * kid (key ID): This is an optional header claim. It is not necessarily included by all OpenID providers. When Salesforce is configured as an OpenID provider, its ID tokens (in JWT format) will contain this optional header claim.

This claim contains an identifier for the key that was used to sign the token. Salesforce (and some other OpenID token providers) use multiple private keys to sign JWTs, and therefore they need to provide a **key ID** (`kid`) to help the recipients identify which public key can be used to verify the signature.

The applications that receive a signed JWT from Salesforce would extract the value of the `kid` header claim and then use it to look up the correct public key from the list published regularly by Salesforce to verify the token’s signature. All this happens transparently, and the user would not be bothered unless the signature is invalid. In such a case, an exception will be thrown.

> Note
> 
> If you go to the page at this public URL, `https://packt.link/qBw4j`, you will see multiple keys listed by Salesforce. Each is identified using a unique key ID.

One thing to keep in mind is that Salesforce, similar to other OpenID providers, can regularly change the keys used to sign their tokens. The client application should never attempt to hardcode `kid`, but instead should attempt to resolve it dynamically, as you learned earlier in this section.

# Session Token

Also known as the session ID, `session token` is a common term used in reference to web applications. A session token is a unique string (normally, a hash value) that identifies a particular session for the currently logged-in user. Session IDs are heavily used on websites, and they are normally stored within a cookie. The session ID can normally be mapped to a key-value dictionary in the application server, which contains information about the logged-in user and what they are authorized to do. If an attacker manages to get hold of a session ID/session token, they would normally be able to impersonate the victim at the target application server.

Therefore, users are always advised to *log out* after they finish working with a particular secure website to destroy their sessions. This is also the reason why session IDs are normally configured to have a short lifetime, particularly with financial services applications (such as online banking) where the session can expire after a very short period (such as five minutes) of idle time.

The token’s TTL can be *fixed* (which means the token will expire after the given time, regardless of whether the user is active) or can be *extended* based on user activity. This means the token will expire only after a specific given period of idle time (where the user is simply not interacting with the server); in this case, it is normally called a **sliding token**.
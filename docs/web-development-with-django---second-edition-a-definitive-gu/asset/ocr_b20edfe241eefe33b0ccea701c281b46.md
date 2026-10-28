After you authorize the authentication application, the authentication provider will redirect you back to the requesting site. The URL that you are redirected to will contain a secret token that the requesting site can use to request your user information in the backend. This allows the requesting site to verify who you are by communicating directly with the authentication provider. After validating your identity using a token, the requesting site can redirect you to your content. This flow is illustrated in a sequence diagram in Figure 15.36:

![](bbox=[0.12, 0.23, 0.82, 0.81])

Figure 15.36 – Third-party authentication flow
By the end of this chapter, you will be able to add an authentication layer to a FastAPI application. This chapter will explain the processes for securing passwords by hashing them, adding an authentication layer, and securing routes from unauthenticated users. In this chapter, we’ll be covering the following topics:

+   - Authentication methods in FastAPI
+   - Securing the application with OAuth2 and JWT
+   - Protecting routes using dependency injection
+   - Configuring CORS

# Technical requirements

To follow along, the MongoDB database component is required. The installation procedures for your operating system can be found in their official documentation. The code used in this chapter can be found at https://github.com/PacktPublishing/Building-Python-Web-APIs-with-FastAPI/tree/main/ch07/planner.

# Authentication methods in FastAPI

There are several authentication methods available in FastAPI. FastAPI supports the common authentication methods of basic HTTP authentication, cookies, and bearer token authentication. Let’s briefly look at what each method entails:

+   - **Basic HTTP authentication:** In this authentication method, the user credentials, which is usually a username and password, are sent via an Authorization HTTP header. The request in turn returns a WWW-Authenticate header containing a Basic value and an optional realm parameter, which indicates the resource the authentication request is made to.
+   - **Cookies:** Cookies are employed when data is to be stored on the client side, such as in web browsers. FastAPI applications can also employ cookies to store user data, which can be retrieved by the server for authentication purposes.
+   - **Bearer token authentication:** This method of authentication involves the use of security tokens called bearer tokens. These tokens are sent alongside the Bearer keyword in an Authorization header request. The most used token is JWT, which is usually a dictionary comprising the user ID and the token’s expiry time.

Every authentication method listed here has its specific use cases as well as its pros and cons. However, in this chapter, we’ll be making use of bearer token authentication.
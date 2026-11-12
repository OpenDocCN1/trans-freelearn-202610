

The ID token itself is digitally signed, and therefore it is easy to validate and confirm whether its contents have been tampered with. As the IDP’s digital signature secures the ID token, and because it contains key information about the user it is generated for (including the federated identifier), it can be used to authenticate this user by other systems (SPs) who trust the IDP. The trust relationship is established using a digital certificate.

OIDC is widely used today; most social networks, such as Facebook, Twitter, Google, and others, support it. The term social sign-on refers to the authentication process where social networks are used as IDPs to log in to other websites. Social sign-on mostly utilizes OIDC. Also, many enterprises use OIDC as their SSO standard.

## Kerberos

Kerberos as an authentication protocol is used over networks (such as a local enterprise network) to provide SSO capabilities. It uses tickets, which are similar in principle to tokens. Kerberos is named after the Greek mythological three-headed dog, Cerberus, who guards the entrance to the underworld. Kerberos is used in several systems, most remarkably, Microsoft Windows. integrated windows authentication (IWA) is a Microsoft product that utilizes Kerberos (it can also work with other protocols such as NT lan manager (NTLM)) to allow Windows users to use AD for SSO.

For instance, imagine that you are an employee of a company that utilizes an AD to manage all its employees’ identities. You arrive at the office in the morning, open your Windows-based laptop, and authenticate to join the domain using your AD credentials (which creates a Kerberos ticket). And now you want to access your company’s instance of Salesforce (which has a unique MyDomain URL). But you want to do that without the need to authenticate again.

Kerberos, IWA, and a tool such as Salesforce Identity Connect, Microsoft ADFS, or Ping Identity (which would facilitate the authentication flow between Salesforce and AD) would all combine to deliver this functionality. The details of setting up Kerberos or IWA are beyond the scope of this book. For the sake of brevity, this book will not detail the Kerberos protocol itself, but you will discover how it is utilized during an SSO flow in Chapter 13, Present and Defend: First Mock.

The standards (SAML, OAuth, OIDC, and Kerberos) utilize and generate tokens as part of their design. Tokens have a crucial role in IAM. They are involved in many different operations and come in different formats and purposes. You will now go through the most common types of tokens that you might come across as a Salesforce Architect.

## Getting to Know the Different Types of Tokens

As a Salesforce Architect, you are expected to lead the activities of designing and implementing IAM strategies for your Salesforce implementation. Tokens are an essential element of the IAM standards, and you need to know more than just their names.

> Note
> The tokens listed here are by no means a complete list of all possible tokens; the list is too long and probably requires a dedicated book.

This is a selected set that Salesforce Architects designing solutions for B2B and B2C are likely to come across.

## Access Token

Access token is a term used by OAuth 2.0 and OIDC. The access token is the most significant token that your applications are after. This is the token that will allow an application (also referred to as a client) to authenticate to a resource server and request resources (such as retrieving specific data) on behalf of the user. The access token can be used to authenticate against web services as well, typically by passing the token itself in the header. The access token can be formatted in multiple forms depending on the system issuing it. For example, Salesforce-issued access tokens take the shape of a normal session ID, which will look similar to the following example:

```
00DR0000000o8tBT!AQwAQCPqzc_HBE59c80QmEJD4rQRRc1GRLvYZEqc80QmEJD4
```

The value of this token cannot be decoded. It is a unique hash and is not supposed to be decrypted or decoded. By itself, it does not contain any information. However, it can be used to access resources on a resource server because a copy of the same token will be cached by the server, usually in a key/value pair dictionary, along with its expiry date. Access tokens have relatively short longevity (such as 30 minutes, 60 minutes, and 120 minutes). The system administrator determines the specific amount of time until session timeout.

Another example is Azure-issued access tokens, which are formatted as JWTs. JWTs contain some standard and custom attributes/payloads (also known as claims).

> Note
> You can use an online tool such as https://packt.link/cPtWd to see a live example of a JWT and its decoded value.

You will come across other JWT-formatted tokens later in this chapter.

## Refresh Token

This is another term used by OAuth 2.0 and OIDC. Refresh tokens are issued for clients in specific use cases (depending on the client type and the authentication flow used). The refresh token normally has greater longevity than the access token. Sometimes, they can be set to never expire (until they get revoked).

The refresh token should be stored in a secure location by the client application and used to get a new access token periodically or whenever needed. You can see refresh tokens in action right now using one of the applications on your smartphone.

Perhaps you have noticed that some apps require you to sign in once, the first time you start the application. You never need to enter your credentials again the next time you open the app, even if you open it after a few days or weeks. This is because the application has been configured to utilize a refresh token. When you authenticated for the first time, the application received both an access token and a refresh token. The access token has a shorter **time to live (TTL)** than the refresh token; it will usually be valid for a few hours. The application will continue to use it as long as it is still valid.

For instance, you opened the application after two weeks. The application will use the cached access token to communicate with the resource server, but will immediately get a response back, indicating that the token has expired. The application will respond by utilizing the refresh token to get a new access token. Once that is done (and you will see that flow later in this chapter), the application will use the newly retrieved access token to communicate with the resource server. All of this is done in a completely transparent way for the user.

## ID Token

This is a token that is exclusive to OIDC. As mentioned before in the OpenID Connect section, the ID token provides a mechanism for the client application to verify the identity of the user for whom this token has been issued. The ID token contains information such as the time of issue and the time of expiry. In addition to data regarding the authenticated user, such as the user's unique identifier (also known as the federated identifier). Depending on the IDP setup, the federated identifier could be a value such as an employee ID, email address, or a different sort of unique identifier.

The ID token also contains information about the client application for which the token was issued. Most importantly, the token will also contain the IDP's digital signature, which can be used to verify the token's contents. The ID token format is based on JWT, which is a data structure based on JSON. Applications can request an ID token using the same OAuth 2.0 flows if the right parameters are passed. For example, in the web server flow, the client application could get an ID token if it included the OpenID scope as part of the passed scope arguments. You will see further details about this later in this chapter.

## JWT

**JWT** is a standard to format data that can be used to assert the identity of a party to other parties as well as asserting a set of information (referred to as claims) that is included in the token itself. ID tokens use the JWT format. JWTs can either be signed (similar to the example given before while describing the ID token), in which case they're referred to as **JSON web signature (JWS)** tokens, or encrypted, in which case they're called **JSON web encryption (JWE)** tokens.


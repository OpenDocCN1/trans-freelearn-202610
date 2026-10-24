# JWT

JWT is a URL-safe method for communicating data. A JWT can be seen as an encrypted string containing a JSON object with a lot of information. It includes an additional structure consisting of a header payload that uses JSON format. JWTs can be encrypted or signed with a Message Authentication Code (MAC). A JWT is created by combining the header and payload JSON, and the whole token is Base64-URL-encoded.

# When to use JWT

JWT is used chiefly on RESTful web services that cannot maintain a client state since JWT holds some information connected to the user. It can provide state information to the server for each request. JWT is utilized in applications that require client authentication and authorization.

# JWT example

Let's have a look at the following JWT example:

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZTsi6IlNlaWppIFZpbGxGxhZnJhb mNhIiwiaWF0IjoxNTE2MjM5MDIyfQ.uhmdFM4ROwnerVam-zdYojURqrgL7WQRBRj-P8kVv6s
```

The JWT in the given example is composed of three parts – we can notice that it is divided with a dot (.) character. The first string is the encoded header, the second string is the encoded payload, and the last string is the signature of the JWT.

The following block is an example of the decoded structure:

```
// Decoded header
{
  "alg": "HS256",
  "typ": "JWT"
}
// Decoded Payload
{
  "sub": "1234567890",
  "name": "Seiji Villafranca",
  "iat": 1516239022
}
// Signature
HMACSHA256(
  base64UrlEncode(header) + "." +
```
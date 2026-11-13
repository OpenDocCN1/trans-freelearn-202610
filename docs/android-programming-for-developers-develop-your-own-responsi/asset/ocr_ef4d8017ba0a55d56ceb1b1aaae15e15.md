```
conn.setRequestMethod(request.mVerb);
// set The HTTP Request Headers
setRequestHeaders(conn,request);
// Allows Receiving data on the response
// conn.setDoInput(true);
// Retrieve the response code
int responseCode = conn.getResponseCode();
// Build the HTTP Response Object
HTTPResponse.Builder builder = new HTTPResponse.Builder()
.setResponseCode(responseCode);
// Fill the HTTP Response Headers
fillHeaders(conn.getHeaderFields(), builder);
// Read the Body from the Connection Input Stream
// using a factory
body = BodyFactory.read(conn.getContentType(),
conn.getInputStream());
// Set the HTTPResponse body
builder.setBody(body);
// Build the HTTP Response Object
response.result = builder.build();
} catch (Exception e) {
response.error = e;
} finally {
if ( conn != null ) {
conn.disconnect();
}
}
return response;
}
```

```
// Write any header to the Request Ex: Accept: text/xml
void setRequestHeaders(HttpURLConnection con, HTTPRequest request )
{
for (Header header : request.mHeaders) {
con.addRequestProperty(header.getName(), header.getValue()));
}
}
```

openConnection will establish a TCP connection with the resource specified in
the URL object. Once the connection is established, and we try to retrieve the
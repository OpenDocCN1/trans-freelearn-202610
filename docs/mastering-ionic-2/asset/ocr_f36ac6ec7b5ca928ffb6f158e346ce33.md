# # CSRF

**Cross Site Request Forgery (CSRF)** and many servers have this security feature. We can also keep this feature in our Ionic-based application. CSRF is a type of attack that occurs when malicious code performs an unwanted action on a trusted site mainly using the user's cookies itself when the user is authenticated. To prevent CSRF with Angular and Ionic you need to set correct headers for CSRF using the specific cookie name. This will keep away hackers from sniffing your cookie session data and making requests pretending on behalf of an authenticated user.

This will set the CSRF request header to the current value of the CSRF cookie for any request type not in `allowedMethods`.

So to enable cookies based auth in Angular you have to take care of another important thing, setting the `withCredentials` flag of AJAX as true. This flag is set on a low level AJAX object, but in Angular we have to configure it with our `Http` method. // **Sample code snippet**

```
```typescript
import { Http, Headers, RequestOptions } from '@angular/http';

sendData() {
  let headers = new Headers();
  headers.append('X-CSRF-Token', token.token);
  let options = new RequestOptions({ headers: headers,
    withCredentials: true });
  this.http.post("URL", <stringified_data>, options)
}
```
```

While many developer face issues such as cookies expiring on restarting the application, if you face a similar issue, make sure your cookies have an expiry set. Actually, browsers clear cookies without an expiry on close.
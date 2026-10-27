
Credentials defaultcreds = new UsernamePasswordCredentials("admin","admin");
objHttpClient.getState().setCredentials(new
AuthScope("localhost",8080, AuthScope.ANY_REALM), defaultcreds);

try {
    // Now, execute the method using HttpClient.
    int statusCode = objHttpClient.executeMethod(objGetMethod);
    if (statusCode != HttpStatus.SC_OK) {
        System.err.println("Method invocation failed: " +
            objGetMethod.getStatusLine());
    }

    // Read the response body.
    byte[] responseBody = objGetMethod.getResponseBody();

    // Print the response body.
    System.out.println(new String(responseBody));

} catch (HttpException e) {
    System.err.println("Http exception: " + e.getMessage());
    e.printStackTrace();
} catch (IOException e) {
    System.err.println("IO exception transport error: " +
        e.getMessage());
    e.printStackTrace();
} finally {
    // Release the method connection.
    objGetMethod.releaseConnection();
}

> Note that the Apache commons client is a legacy project now and is not being developed anymore. This project has been replaced by the Apache HttpComponents project in HttpClient and HttpCore modules. We have used HttpClient from Apache commons client here to get an overall understanding.

Some of the other options that you can use to invoke web scripts from a Java program are mentioned in subsequent sections.

# URLConnection

One option to execute web script from Java program is by using java.net.URLConnection. For more details, you can refer to http://docs.oracle.com/javase/tutorial/networking/urls/readingWriting.html.

# Apache HTTP components

Another option to execute web script from Java program is to use Apache HTTP components that are the latest available APIs for HTTP communication. These components offer better performance and more flexibility and are available in httpclient-*.jar and httpcore-*.jar. These JARs are available at the tomcat\webapps\alfresco\WEBINF\lib location inside your Alfresco installation directory. For more details, refer to https://hc.apache.org/httpcomponents-client-4.3.x/quickstart.html to get an understanding of how to execute HTTP calls from a Java program.

# RestTemplate

Another alternative would be to use org.springframework.web.client.RestTemplate available in org.springframework.web-*.jar located at tomcat\webapps\alfresco\WEB-INF\lib inside your Alfresco installation directory. If you are using Alfresco Community 5, the RestTemplate class is available in spring-web-*.jar. Generally, RestTemplate is used in Spring-based services to invoke an HTTP communication.

# Calling web scripts from Spring-based services

If you need to invoke an Alfresco web script from Spring-based services, then you need to use RestTemplate to invoke HTTP calls. This is the most commonly used technique to execute HTTP calls from Spring-based classes. In order to do this, the following are the steps to be performed. The code snippets are also provided:

+   1. Define RestTemplate in your Spring context file:
+   ```
+   <bean id="restTemplate" class="org.springframework.web.client.RestTemplate" />
+   ```

+   2. In the Spring context file, inject restTemplate in your Spring class as shown in the following example:
+   ```
+   <bean id="httpCommService" class="com.test.HTTPCallService">
+       <property name="restTemplate" value="restTemplate" />
+   </bean>
+   ```

+   3. In the Java class, define the setter method for `restTemplate` as follows:

```
private RestTemplate restTemplate;
public void setRestTemplate(RestTemplate restTemplate) {
    this.restTemplate = restTemplate;
}
```

+   4. In order to invoke a web script that has an authentication level set as user authentication, you can use RestTemplate in your Java class as shown in the following code snippet. The following code snippet is an example to invoke the hello world web script using RestTemplate from a Spring-based service:

```
// setup authentication
String plainCredentials = "admin:admin";
byte[] plainCredBytes = plainCredentials.getBytes();
byte[] base64CredBytes = Base64.encodeBase64(plainCredBytes);
String base64Credentials = new String(base64CredBytes);

// setup request headers
HttpHeaders reqHeaders = new HttpHeaders();
reqHeaders.add("Authorization", "Basic " + base64Credentials);
HttpEntity<String> requestEntity = new
HttpEntity<String>(reqHeaders);

// Execute method
ResponseEntity<String> responseEntity = restTemplate.
exchange("http://localhost:8080/alfresco/service/
helloworld?name=Ramesh", HttpMethod.GET, requestEntity, String.
class);
System.out.println("Response:"+responseEntity.getBody());
```

# Invoking a web script from Alfresco Share

When working on customizing Alfresco Share, you will need to make a call to Alfresco repository web scripts. In Alfresco Share, you can invoke repository web scripts from two places. One is the component level presentation web scripts, and the other is client-side JavaScript.

# Calling a web script from the presentation web script JavaScript controller

Alfresco Share renders the user interface using the presentation web scripts. These presentation web scripts make a call to the repository web script to render the repository data. Repository web script is called before the component rendering file (for example, `get.html.ftl`) loads.

In out-of-the-box Alfresco installation, you should be able to see the components' presentation web script available under `tomcat\webapps\share\WEB-INF\classes\alfresco\site-webscripts`.

When developing a custom component, you will be required to write a presentation web script. A presentation web script will make a call to the repository web script. You can make a call to the repository web script as follows:

```
var response = remote.call("url of web script as defined in description document");
var obj = eval('(' + response + ')');
```

In the preceding code snippet, we have used the out-of-the-box available remote object to make a repository web script call. The important thing to notice is that we have to provide the URL of the web script as defined in the description document. There is no need to provide the initial part such as host or port name, application name, and service path the way we use while calling web script from a web browser. Once the response is received, web script response can be parsed with the use of the `eval` function.

In the out-of-the-box code of Alfresco Share, you can find the presentation web scripts invoking the repository web scripts, as we have seen in the previous code snippet. For example, take a look at the `main()` method in the `site-members.get.js` file, which is available at the `tomcat\webapps\share\components\site-members` location inside your Alfresco installed directory. You can take a look at the other JavaScript controller implementation for out-of-the-box presentation web scripts available at `tomcat\webapps\share\WEB-INF\classes\alfresco\site-webscripts` making repository web script calls using the previously mentioned technique.

> When specifying the path to provide references to the out-of-the-box web scripts, it is mentioned starting with `tomcat\webapps`. This location is available in your Alfresco installation directory.

# Invoking a web script from client-side JavaScript

The client-side JavaScript control file can be associated with components in Alfresco Share. If you need to make a repository web script call, you can do this from the client-side JavaScript control files generally located at tomcat\webapps\share\components. There are different ways you can make a repository web script call using a YUI-based client-side JavaScript file. The following are some of the ways to invoke web script from client-side JavaScript files. References are also provided along with each of the ways to look in the Alfresco out-of-the-box implementation to understand its usage practically:

+   - Alfresco.util.Ajax.request: Take a look at tomcat\webapps\share\components\console\groups.js and refer to the _removeUser function.
+   - Alfresco.util.Ajax.jsonRequest: Take a look at tomcat\webapps\share\components\documentlibrary\documentlist.js and refer to the onOptionSelect function.
+   - Alfresco.util.Ajax.jsonGet: To directly make a call to get web script, take a look at tomcat\webapps\share\components\console\groups.js and refer to the getParentGroups function.
+   - YAHOO.util.Connect.asyncRequest: Take a look at tomcat\webapps\share\components\documentlibrary\tree.js and refer to the _sortNodeChildren function.

> In alfresco.js located at tomcat\webapps\share\js, the wrapper implementation of YAHOO.util.Connect.asyncRequest is provided and various available methods such as the ones we saw in the preceding list, Alfresco.util.Ajax.request, Alfresco.util.Ajax.jsonRequest, and Alfresco.util.Ajax.jsonGet can be found in alfresco.js. Hence, the first three options in the previous list internally make a call using the YAHOO.util.Connect.asyncRequest (the last option in the previous list) only.

# Calling a web script from the command line

Sometimes while working on your project, it might be required that from the Linux machine you need to invoke a web script or create a shell script to invoke a web script. It is possible to invoke a web script from the command line using cURL, which is a valuable tool to use while working on web scripts.

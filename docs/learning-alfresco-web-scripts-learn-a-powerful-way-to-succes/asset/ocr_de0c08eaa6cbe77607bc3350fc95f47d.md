Let's extend our first web script to add a controller to get an understanding about how the controller populates and passes data to FreeMarker template and how FreeMarker template uses the model data to generate the response:

+   1. Log in to Alfresco Share UI.
+   2. Click on the **Repository** link from the top header.
+   3. Go to **Data Dictionary | Web Scripts Extensions**.
+   4. Edit the descriptor file `helloworld.get.desc.xml` as follows:
+   ```
+   <webscript>
+     <shortname>Hello World</shortname>
+     <description>First webscript Hello world</description>
+     <url>/helloworld?name={argumentName}</url>
+     <authentication>user</authentication>
+   </webscript>
+   ```
+   5. Create a new file named `helloworld.get.js` with content as follows:
+   ```
+   model.email="Ramesh.chauhan@cignex.com";
+   ```
+   6. Edit the FreeMarker template `helloworld.get.html.ftl` as follows:
+   ```
+   <html>
+     <body>
+       <p>Hello! ${args.name}.</p>
+       <p>email address: ${email}</p>
+     </body>
+   </html>
+   ```
+   7. Go to `http://localhost:8080/alfresco/service/index` and click on the **Refresh Web Scripts** button.
+   8. Now, hit the URL `http://localhost:8080/alfresco/service/helloworld?name=Ramesh` to execute the web script. It will ask for user credentials. Provide user credentials and you should see the response as:
+   **Hello! Ramesh.**
+   **email address: Ramesh.chauhan@cignex.com**

This is how we can have a controller added in a web script in a simple way. In the preceding example, we hard-coded the e-mail address to be returned in the web script response. We did not have any interaction with the Alfresco repository. We just populated the model object and passed it to the FreeMarker template from the server-side JavaScript controller.
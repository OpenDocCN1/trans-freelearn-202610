To build our full stack application, we'll need two parts:

+   - **The frontend**: This is the application that runs in the client's web browser on the client's machine. Historically, this was accomplished through the use of HTML pages rendered on the server and then returned to the client. However, browsers may now run fully fledged JavaScript applications (also known as **client-side apps**), which do the majority of the processing on the client's browser and rely on servers just to supply the application's initial files and data. Simply put, the frontend is what presents the user interface with which users interact.
+   - **The backend**: This is the server-side application that will handle HTTP requests, perform some processing logic, and return responses to the browser (HTML and/or JSON data).

We'll use tools such as Lerna to organize our application's code utilizing a monorepo approach. In the context of software development, monorepo simply refers to using a single source code repository (typically version controlled using Git) for all of our applications (the server, client, and any shared libraries).

Let's take a high-level look at how our application will be delivered to the client's machine:

+   1. First, the client will make a request by putting your application domain name into the address bar of the browser.
+   2. The server will intercept the requests and process the HTML document containing the Angular app.
+   3. The client's browser will begin downloading all of the JavaScript and CSS files required to run the Angular application.
+   4. Any initial requests to the server for data, such as posts, will be sent by the Angular application and rendered in the user interface.
+   5. The other requests will be made to the server after the user begins engaging with the application.

This process provides a high-level overview of how our application operates. We'll go through these stages in greater depth in the next chapters.

After quickly discussing the full stack and monorepo concepts, let's have a look at the technologies and tools we'll be using to build our application.
After this you should have a folder structure like this one:

![](bbox=[0.27, 0.13, 0.67, 0.48])

Now we are ready to build our application for the first time.

# Building the application for the first time

To create our application, we must be able to serve at least two static files, the index.html, which is the HTML page, and the bundle.js file, which contains all the logic for the application written in JavaScript.

Serving the static file using express is extremely easy. To do so, let's change the index.js file inside the src folder (src/index.js) where we implemented our express server, and paste the following code:

```
app.use(express.static('static'));
```

This code tells express to serve all files in the static folder as static files; now we will be able to see our page for the first time. To do this, run your application with:

```
node src/index.js
```
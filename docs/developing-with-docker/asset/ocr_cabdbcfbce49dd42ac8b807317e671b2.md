Ok, it's working as intended. Let's proceed to the server part-the web application needs to be served. Of course, our simple application works by running the HTML file directly in the web browser. If you decide to include more Angular features later, for example routing, it will not work by opening directly. Also, the server usually completes the application by responding to HTTP requests, such as REST calls for example. The response taken from those calls can be then used on the static Angular pages. We could use any programming language we like, for example Java and Spring Boot with docker-maven plugin as we did in the previous example. This time, to present the manual way of packaging an application into Docker container, we are going to use node.js runtime as a backend for our application. To get the required node.js dependencies, we will need to create package.json file with all the required dependencies:

```
{
  "name": "hello-world-docker",
  "main": "index.js",
  "dependencies": {
    "express": "^4.12.3",
    "bower": "1.7.9"
  }
}
```

The package.json file, the same as bower.json, needs to be placed in the root directory of our project. If the file is ready, we can run the npm i»¿ install command to download the needed dependencies, which will be Express and Bower in our case. Execute the following command:

```
npm install
```

After a while, the node_modules folder will be created. It will contain the dependencies mentioned in the package.json file and our application will use them. The entrypoint»¿ for our node.js application will be a simple and basic HTTP web server, serving static content from the frontend directory. Let's name it app.js and fill it out with this content:

```
'use strict';
process.env.NODE_ENV = process.env.NODE_ENV || 'development';
var express = require('express');
var http = require('http');
var path = require('path');
var app = express();
var config = {
  port: 8080,
  root: path.normalize(path.join(__dirname, '../frontend'))
}
app.use(express.static(config.root));
```
In the previous code, we used compojure.core/defroutes to define a context as /hello and liberator.core/resource defines a resource.

Then, let's define a handler and run it. ring.middleware.params/wrap-params parses query strings for requests and form bodies:

```
(def handler
  (-> app
      wrap-params))
;;=> #'liberator-example.example/handler
```

Then, start the server and make requests from your browser:

```
(def server (run-jetty handler {:port 3000 :join? false}))
;;=> #'liberator-example.example/server
```

Open your browser with http://localhost:3000/hello. You can see the following string in your browser:

![](bbox=[0.06, 0.44, 0.68, 0.6])

To make requests from the command line, use the curl command:

```
$ curl -X GET http://localhost:3000/hello
```

The next image shows you how the curl command works:
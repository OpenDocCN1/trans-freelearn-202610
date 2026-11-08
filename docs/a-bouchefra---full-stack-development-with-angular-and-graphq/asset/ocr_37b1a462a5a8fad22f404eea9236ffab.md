# Sending queries with Apollo Studio

Make sure your server is still running without any errors and then go to the http://localhost:8080/graphql address with your web browser. You should be presented with an interface that's similar to the following:

![](bbox=[0.13, 0.2, 0.87, 0.48])

Figure 2.1 – Query your server

From this interface, you can click on the Query your server button to access Apollo Studio, where you can send requests to your API endpoint.

In the left panel, write the following GraphQL query to send a message query:

```
```
query {
  message
}
```
```

Click on the Run button. You should get the following output in the right-hand panel:

```
```
{
  "data": {
    "message": "It works!"
  }
}
```
```

The following screenshot shows Apollo Studio, along with our query and response:
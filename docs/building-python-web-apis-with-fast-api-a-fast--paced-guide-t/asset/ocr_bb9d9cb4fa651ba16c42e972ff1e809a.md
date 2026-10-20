![](bbox=[0.115, 0.095, 0.835, 0.545])

Figure 7.5 – Request body for updated sign-in route

Let's sign in to verify that the route works properly:

```
$ curl -X 'POST' \
'http://0.0.0.0:8080/user/signin' \
-H 'accept: application/json' \
-H 'Content-Type: application/x-www-form-urlencoded' \
-d 'grant_type=&username=reader%40packt.com&password=exemplary&scope=&client_id=&client_secret='
```
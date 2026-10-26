```
```json
"post": "Ionic is opensource SDK or hybrid apps"
},
"123457": {
  "title": "welcome of ionic 2",
  "post": "Ionic is opensource SDK or hybrid apps"
}
}
```
```

Now we have stored the data inside Firebase and the next step is to fetch the data and joining back. Joining flattened data is a bit difficult but this approach comes with a lot of performance and modularity. Let's see how we can fetch the above data and join it.

As Firebase uses web sockets performance will be high while fetching the data this way and Firebase internally do optimization on incoming and outgoing requests.

Firebase also provides a set of security rules for securing our data and help determine who has read and write permission for the data set. All the security rules in dashboard inside the Security & Rules tab. Inside our application our application data is user-defined and associated to the user, so we have set the rules accordingly:

![](bbox=[0.008, 0.364, 0.992, 0.73])

There are three types of rule:

+   - `.read` - when data is allowed to read be by users
+   - `.write` - when data is allowed to be written
+   - `.validate` - defines correctly formatted data and validates before pushing to the database

Security rules are in the JSON structure as you see in the previous screenshot and this makes it easier to understand and set rules. You will be associated with user, so we have set the rules accordingly. In our application we first need to store the user-related data during the signup process such as the name, e-mail, and other details:

```
```json
{
```
```
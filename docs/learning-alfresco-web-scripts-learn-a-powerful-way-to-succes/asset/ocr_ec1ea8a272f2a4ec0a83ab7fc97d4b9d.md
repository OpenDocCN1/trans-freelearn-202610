```
```javascript
var description = node.properties["cm:description"];
var description = node.properties["{http://www.alfresco.org/model/content/1.0}description"];
```
```

We just saw four different ways you can read the property of a node. If you have some custom property, you should use one of the last two options.

# Logging the property value

Now before modifying the description property, you want to log the older value for reference. Using the logger root object, you can simply invoke the log method of the logger root object as follows:

```
```javascript
logger.log("before change description value : " + description);
```
```

We used the logger root object and invoked the log method to log the property value. Each time logging is required to be added in the JavaScript controller, you can use the `logger.log()` method.

# Modifying property of a node

Now, you want to modify the `description` property. Again with the help of the ScriptNode API, you can easily perform it as follows:

```
```javascript
node.properties.description = "change in description";
```
```

We just set a new value to the `description` property. You can also use the other three ways we saw to access the property and set a new value for it.

# Getting the current username and e-mail

You might be interested to know which user has invoked the web scripts. For this, you might want to log the username and e-mail address of the user who executed the web script as follows:

```
```javascript
logger.log(person.properties.username + " --- " + person.properties.email + "--- executed webscript");
```
```

We used the root object `person`, which basically is `ScriptNode`. Also, we fetched the username and e-mail properties for a user who is currently running the web script. You can trace through the logs and find out which users executed the web script; this might be very useful at some point in time later.
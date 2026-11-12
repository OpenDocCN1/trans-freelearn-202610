Finally, we can build our custom Modernizr feature detection script using this command:

```
modernizr -c modernizr-config.json
```

You can also use the -d scripts/modernizr.min.js flag to change the installation destination of the Modernizr script to the scripts folder. Let's create an HTML document that includes this script:

```
<!DOCTYPE HTML>
<html>
<head>
    <title>Feature Detection using Modernizr</title>
    <script src="scripts/modernizr.min.js"></script>
</head>
<body>

<script>
/* here we go * /</script>

</body>
</html>
```

The preceding code shows how we included the Modernizr script on the page. Note that, besides using the command-line tool of Modernizr and the configuration file, you may also use a plugin for your favorite task runner (such as Gulp or Grunt) or simply download your custom build from the Modernizr website.

# Using Modernizr in JavaScript

Let's try Modernizr's JavaScript API; it exposes all available features directly as properties on the global Modernizr object. Checking for SMIL support is as easy as the following code snippet:

```
if (Modernizr.smil) {
    console.log('SMIL is available');
}
```

This is great; we can now easily check in JavaScript whether all our features are available and easily adapt the application and features to the target browser of the user.

> The complete list of available features is provided on the Modernizr website at https://modernizr.com/docs/#features.
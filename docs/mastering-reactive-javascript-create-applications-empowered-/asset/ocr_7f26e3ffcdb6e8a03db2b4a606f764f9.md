```
"author": "",
"license": "ISC",
"dependencies": {}
}
```

OK, now we have our test project. In this project we will do a timer implementation–basically, what we want is to print the current time every second. We will do it with and without the functional reactive programming library, so first let's implement it in the traditional way.

Create a file called traditionalInterval.js and paste the following code inside it, and save:

```
setInterval(
    () => console.log(new Date())
,1000);
```

This code uses the function setInterval () to call our function every 1000 milliseconds (every second). Now, run this program using the following command in your terminal:

```
node ./traditionalInterval.js
```

If you did it right it will print the current date every second in your terminal, like this:

```
2016-10-30T20:28:22.778Z
2016-10-30T20:28:23.796Z
2016-10-30T20:28:24.803Z
2016-10-30T20:28:25.808Z
2016-10-30T20:28:26.809Z
2016-10-30T20:28:27.815Z
2016-10-30T20:28:28.820Z
```

To stop your program just hit Ctrl + C.

Now let's see how we can implement the same program using bacon.js for functional reactive programming. First, we need to install bacon.js on our project, as described in the Installation of tools section and also described here:

```
npm i baconjs@0.7.88 --save
```

With bacon.js installed in our project we are ready to implement our program. First create a file called baconInterval.js and then, as we will require the library in our project, open this file in a text editor and paste the following code:

```
var Bacon = require("baconjs");
```
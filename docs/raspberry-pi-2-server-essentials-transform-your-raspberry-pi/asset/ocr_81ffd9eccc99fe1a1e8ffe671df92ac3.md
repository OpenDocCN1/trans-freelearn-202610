We can now run this basic node server, which provides a simple API text response with the current core CPU voltage. You can do a AJAX GET request at http://raspberrypi/node/core/voltage, and since this is a GET, you can simply test it by entering this address into your browser after starting the server:

```
npm index.js
```

Now, you can update your index.html file at /var/www/html/web and use jQuery to do a simple GET and update an element on your page. The result varies between 1.2 and 1.3125 (the full sample is provided in the code library):

```
$.get('/node/core/voltage', function(data) {
  $('#cpuv oltage').text(data);
});
```

# .NET and ASP.MVC

To work with Microsoft's .NET framework, we first need to install the latest version of Mono. The typical way is to compile Mono from the Git source, but it takes roughly 6 hours to complete on Raspberry Pi 2. I have tried to come up with a cross-compiling tutorial, but I have not managed to get anything simple or robust enough to demonstrate in this book.

Unfortunately, at the time, the Wheezy and Jessie repositories only have versions that do not support .NET 5 fully. A faster and better way is to use the newer versions maintained by the official Xamarin repository.

These commands will add the official Xamarin repository, update any required packages, and finally, install Mono, which requires about 300 MB of space and takes about 10 minutes to complete. Type in the following commands:

```
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF
```

```
echo "deb http://download.mono-project.com/repo/debian wheezy main" | sudo tee /etc/apt/sources.list.d/mono-xamarin.list
```

```
sudo apt-get update && sudo apt-get upgrade
```

```
sudo apt-get install mono-complete
```
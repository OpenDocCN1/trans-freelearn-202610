Arduino board. For example, for the **On** button, we put a HIGH state on the relay pin, as shown in the following code:

```
if (clicked_id == "1"){
    $.get( "command.php", {command: "/digital/7/1"} );
}
```

Finally, we have to look at the PHP file that will actually send the command to the board. We first need to get the command that was sent by the JavaScript file and store it in a variable:

```
$command = $_GET['command'];
```

Then, we need to set the URL to which we will send the command. To send the command to the board, we will use a PHP module called curl, which will do exactly the same as what we just did from the web browser.

At this point, you will have to modify the IP address defined in the PHP file and replace it with the IP address of your board. The following piece of code does just that:

```
$service_url = 'http://192.168.1.103' . $command;
$curl = curl_init($service_url);
```

Then, we can actually perform the cURL call using the following piece of code:

```
curl_setopt($curl, CURLOPT_IPRESOLVE, CURL_IPRESOLVE_V4 );
curl_setopt($curl, CURLOPT_CONNECTTIMEOUT, 0.5);
$curl_response = curl_exec($curl);
curl_close($curl);
```

# Note

The code for this section can be found in the GitHub repository for this chapter at [https://github.com/openhomeautomation/arduino-networking/tree/master/chapter4.](https://github.com/openhomeautomation/arduino-networking/tree/master/chapter4.)

We can now test the interface. Make sure that all the files of this interface are stored inside a folder at the root of your web server folder, and also make sure that the web server is running. Then, go to this folder via the localhost path in your browser and open the HTML file. You should see the following screenshot:

![](bbox=[0.061, 0.696, 0.663, 0.87])

Try to click on one of the buttons: you should hear that the relay instantly switches
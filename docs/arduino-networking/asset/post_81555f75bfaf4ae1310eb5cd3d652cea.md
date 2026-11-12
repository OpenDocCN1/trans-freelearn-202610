
## Plotting the data locally

To end the chapter, we are going to see how to plot data measured by the Arduino board. To do so, we will modify the Arduino sketch a little bit, and then use part of the code from the previous chapter to plot the data right in your web browser.

First, we are going to modify the Arduino sketch so that it returns data in a more useful format; in the present case, the JSON format. In place of the code responsible to print out the measurements, we are going to simply print the data in the JSON format. This is done using the following piece of code:

```
client.println("HTTP/1.1 200 OK");
client.println("Content-Type: application/json");
client.println("Connection: close");
client.println();
client.print("{\"temperature\": ");
client.print(temp);
client.print(", \"humidity\": ");
client.print(hum);
client.println("}");
```

We can now quickly test this project. Upload the code to the Arduino board again, open your web browser, and go to the same IP address you used before. You should see the following line printed on the page:

```
{"temperature": 24, "humidity": 36}
```

Now, we also have to build the server-side code to plot the data. The code is very similar to the code of the previous chapter, so I will only detail the differences between the two projects. In the previous chapter, it was the board that was trying to reach out the server running on your computer, and the datalogger.php file was handling the requests. In this chapter, we are going to do the reverse and call the board from the web server running on your computer. To do so, we are going to use a module from PHP called cURL to make GET requests to a given URL; in this case, the URL of the Arduino board.

The first thing we have to do is make changes to the datalogger.php file and add the URL of the Arduino board, as shown in the following line of code:

```
$url = 'http://192.168.1.103';
```

Start the cURL call, as shown in the following line of code:

```
$curl = curl_init();
```

We also have to set the options of the cURL call. We want the call to return the data from our Arduino web server, and we also want the URL to be the one we defined before. This is done using the following code:

```
curl_setopt_array($curl, array(
    CURLOPT_RETURNTRANSFER => 1,
    CURLOPT_URL => $url,
));
```

We can now execute the command, as shown in the following line of code:

```
$resp = curl_exec($curl);
```

Close the cURL call, as follows:

```
curl_close($curl);
```

We get the answer in a variable that contains the data in a string. In order to open it with PHP, we need to convert it to the JSON format first, and then extract the temperature and humidity fields. This is done using the following piece of code:

```
$json = json_decode($resp, true);
$temperature = intval($json["temperature"]);
$humidity = intval($json["humidity"]);
```

The rest of the file is strictly identical to the code we developed in the previous chapter.

Now, we also need to make a small modification in the plot.html file. In the previous chapter, the data was automatically logged inside the database as the Arduino board was constantly sending data to the server. Here, we need to make the call on the server side. This is done by adding the following small piece of code just before the code that reads out the data from the database:

```
$.ajax({
  url: "datalogger.php",
  type: "GET",
});
```

This means that just before the data is read from the database to be plotted, we are calling the Arduino board to get the temperature and humidity measurements.

## Note

All the code for this section can be found inside the GitHub repository of this chapter at https://github.com/openhomeautomation/arduino-networking/tree/master/chapter3.

We can now test this part of the chapter. Note that the code from the GitHub repository includes an example database so you can test the project right away, but I recommend deleting the file just before testing the project, so you will plot your own measured data while testing the code.

You can just put all the code for the plotting part inside a folder at the root of your web server. Then, open the plot.html file via the localhost URL in your web server. You should immediately see that data is being plotted inside your web browser. After a while, you should get a graph similar to the following graph:

![](img/b454031c864977fe997c5feb0b3f9b63_94_0.png)

If you can see this plot in your browser, congratulations! This means you can perform remote measurements from the web server running on your Arduino board.

If this is not the case, there are several things you can check. This first thing you can check is making sure that the Arduino board correctly displays the data in a JSON format. To do so, you can simply enter the address of your board in a browser. Then, check whether your web server is running on your computer and that you have placed all the files from the project inside a repository in your web server's main folder. Finally, make sure that you are accessing the HTML file from the localhost URL in your browser and not by clicking on it inside your file explorer.

## Summary

Let's summarize what we did in this chapter. We created an independent measurement station with Arduino that automatically measures and serves the data in some way. First, we logged the data locally on a SD card and then we served this data using a web server running on the Arduino board.

The following are the major takeaways of this chapter:

- 1. First, we built the required hardware for this chapter, and inserted the SD card into the Ethernet shield.
- 2. Then, in the first part of the project, we logged the measured data on the SD card and used a remote time server to automatically get the measurement times.
- 3. Finally, we built a new Arduino sketch to have a web server running on the Arduino board. We then modified the plotting code from the previous chapter so it can call the server on the Arduino board to get and plot the measured data.

There are, of course, many ways to improve this project. You can add more sensors to the project and either log this additional data on a SD card or serve it on a web page using the Arduino web server. You can also have many of these boards in your home or even outside and make your computer poll all these Arduino servers at a regular interval to get their measurements.

In the next chapter, we are going to work again with this idea of creating an independent system with the Arduino Ethernet shield. We are going to connect a relay to the Arduino board, so you can switch a lamp on and off remotely. This project will come in two flavors. First, we will make a sketch to control the relay remotely via your web browser. In the second part of the chapter, we will use a special library so the relay can be controlled from anywhere in the world.

## Chapter 4. Controlling Objects from Anywhere

In this chapter, we are going to do something different to what we did in the other chapters. What we did so far was measure some data on the Arduino board and transmit this data back to the network using the Ethernet shield. In this chapter, we are going to control a device instead of measuring data.

To do so, we will use a relay connected to the Arduino board with the Ethernet shield and control this relay remotely. We are also going to connect a set of power cables to this relay so you can directly plug a lamp into it so that it can be controlled remotely.

In the first part of the chapter, we are going to control this relay remotely within your local network. In the second part of the chapter, we will use a dedicated web service to control this relay from anywhere in the world.

The following will be the major takeaways from this chapter:

- First, we are going to see how to choose the different components for this project, including the relay and cables to connect a device to the relay such as a lamp.
- Then, we will assemble the different hardware components, connect the relay to the Arduino board, and the power cables to the relay.
- After that, we will write an Arduino sketch to test whether the relay is working correctly and has been correctly connected to the circuit.
- When we are sure that the relay is working, we will write an application using the Ethernet shield along with the relay. We are going to write a sketch to control the relay via Ethernet and then build an interface to control the relay from your web server.
- Finally, in the last section of the chapter, we are going to use a dedicated web service to control the relay from anywhere in the world. We are also going to adapt the interface we just created to control the relay from anywhere.


Serial.print("IP address: ");
Serial.println(Ethernet.localIP());

In the loop() function of the sketch, the first step is to take measurements from the DHT11 sensor:

```
float h = dht.readHumidity();
float t = dht.readTemperature();
```

Convert these measurements into strings:

```
String temp = String((int) t);
String hum = String((int) h);
```

For debugging purposes, we also print these values on the serial port. We'll check later whether these values are correct when testing the sketch:

```
Serial.println("Temperature: " + temp);
Serial.println("Humidity: " + hum);
```

Now, we are actually going to send the data to the server. Don't worry about understanding what the server-side code does for now, as we'll deal with that later. First, we have to connect to the server running on your computer:

```
if (client.connect(server, 80)) {
 if (client.connected()) {
 Serial.println("connected");
}
}
```

If this is successful, we can make the request. As in Chapter 1, Discover the Arduino Ethernet Shield, we are going to use a standard GET request, and to pass the temperature and humidity measurements as arguments. At this point, you will also need to enter the IP address of your computer. This is all done using the following piece of code:

```
client.println("GET /datalogger/datalogger.php?temp=" + temp + "&hum=" + hum + " HTTP/1.1");
 client.println("Host: 192.168.1.100");
 client.println("Connection: close");
 client.println();
```

You can see that the code calls a filename datalogger.php, which we are going to examine in the next section.

Then, after the request is made, we can read the answer from the server:

```
while (client.connected()) {
 while (client.available()) {
 char c = client.read();
 Serial.print(c);
 }
}
```

Next, we can close the connection if the client is not connected to the server anymore:

```
if (!client.connected()) {
 Serial.println();
 Serial.println("disconnecting.");
 client.stop();
}
```

```
delay(1000);
```

## Note

You can find all the code for this section in the GitHub repository of this chapter:

https://github.com/openhomeautomation/arduino-networking/tree/master/chapter2

Now, if we just uploaded the code to the Arduino board, not much would have happened, since we didn't do anything on the server side. The Arduino board would make the request, but as the request file doesn't exist on the server, the server would return an error code.

So first, we need to build a file on the server that will handle the request from the board and log the data somewhere. That's exactly what we are going to do next.

## Log incoming data in a database

In this section, we are going to use PHP to build the server-side part of the project. If you are a complete novice in PHP, I recommend the following resource to learn the basics of the language:

[http://php.net/manual/en/tutorial.php](http://php.net/manual/en/tutorial.php)

First, we are going to see the content of the datalogger.php file. This file will handle the requests coming from the Arduino Ethernet shield, log the data in a database, and answer with a simple message. Note that this file has to be in a folder named datalogger on your web server. We will see the important parts of the code. To get the complete code for this section, please refer to the GitHub repository of the chapter. Note that all the PHP code should be between the <?php ... ?> tags.

The file starts by receiving the data from the GET request sent by the Arduino Ethernet shield:

```
$temperature = intval($_GET["temp"]);
$humidity = intval($_GET["hum"]);
```

We also instantiate the connection with the SQLite database:

```
$db = new SQLite3('database.db');
```

Then, we need to give some structure to the database if the database file is brand new. If you are not familiar with the SQL commands, I invite you to visit the following link:

[http://www.cs.utexas.edu/~mitra/csFall2013/cs329/lectures/sql.html](http://www.cs.utexas.edu/~mitra/csFall2013/cs329/lectures/sql.html)

We are going to create four different columns inside the database: a unique ID that will be automatically incremented by SQLite, a timestamp to know when the measurement was made, and the temperature and humidity data. This is done using the following piece of code:

```
$db->exec('CREATE TABLE IF NOT EXISTS measurements (id INTEGER PRIMARY KEY,
timestamp TIMESTAMP DEFAULT CURRENT_TIMESTAMP NOT NULL, temperature
INTEGER, humidity INTEGER);');
```

Note that if you are using more sensors (for example, if you are also measuring the ambient light level), you can add more fields here. Simply add a new field to the list, for example: pressure INTEGER.

Now, we can actually insert the data as a new row in the database. Since SQLite automatically adds the ID and timestamp fields, we just need to insert the data concerning the temperature and humidity:

```
$db->exec("INSERT INTO measurements (temperature, humidity) VALUES
('$temperature', '$humidity')");
```

If you need to insert more data into the database, for example, if you have another measurement to log, you can simply extend this command to insert more data into the SQLite database.

## Tip

This simple code illustrates how easy it is to insert data into the SQLite database. However, it leaves our server exposed to a security issue known as the SQL injection. Since our project is made for your own local network only, this is not really an issue here. However, if you plan to deploy the project online, I recommend modifying the code to solve this security problem. The following link will give you more information about this issue [http://www.tutorialspoint.com/sqlite/sqlite_injection.htm](http://www.tutorialspoint.com/sqlite/sqlite_injection.htm).

Finally, we reply to the Arduino board with a simple message:

```
echo "Data received";
```

Now, we haven't actually talked about the database itself yet. There is a database.db file included as an example in the code for this chapter, but I recommend you simply delete it and try to make your own database. You can simply go to the directory in a terminal where all files of the project are located and type:

```
sqlite3 database.db
```

## Tip

If you are using Windows, I recommend using Console as a terminal. You can get it at the following link [http://sourceforge.net/projects/console/](http://sourceforge.net/projects/console/).

This command will create your database file. You can now again exit the database file by typing the following command:

```
.exit
```

Note that if you are under Linux, you might have to change the permissions on the database and the folder that contains the project. To do so, go the project's folder via a terminal and type:

```
chmod 777 database.db
```

Now, it is time to make a first test of the project. Make sure that all the files of the server are located in a folder called datalogger on your web server. You can now upload the Arduino sketch to your board and open the Serial monitor. You should first see that the sketch is connecting to your network and receiving an IP address as follows:

```
IP address: 192.168.1.103
```

Then, it should enter the first iteration of the loop() function, and print out the temperature and humidity measurements:

```
Temperature: 21
Humidity: 37
```

After this step, you should see that the sketch is connected to your server, and that the server is answering with a standard 200 OK answer:

```
connected
HTTP/1.1 200 OK
```

```
Date: Mon, 19 May 2014 08:09:57 GMT
Server: Apache/2.2.23 (Unix) mod_ssl/2.2.23 OpenSSL/0.9.8y DAV/2 PHP/5.4.10
X-Powered-By: PHP/5.4.10
Content-Length: 13
Connection: close
Content-Type: text/html
```

Also, at the end of this answer, you should see the confirmation message that we defined in the PHP file:

```
Data received
disconnecting.
```

If you are seeing these messages, congratulations! It means that your Arduino Ethernet shield is successfully communicating with the web server running on your computer, and is actually logging some data.

If this is not the case, first check your hardware connections. Make sure the DHT11 sensor is correctly wired with the Arduino board, and that the Ethernet cable is plugged into the shield and your Internet router. Don't hesitate to redo Chapter 1, Discover the Arduino Ethernet Shield, to make sure that your Ethernet shield is working properly.

Also, make sure that the web server is active on your computer, and that all the files of the project (especially the `datalogger.php` file) are located in a folder named `datalogger` inside your web server folder. You can test this independently from the Arduino Ethernet shield. Simply go to your favorite web browser, and type:

```
localhost/folder_of_the_project/datalogger.php?temp=20&hum=40
```

This will log some fake data inside the database, so you can be sure that the web server is running correctly.

We are now going to check on the server side whether the data was recorded correctly. And we are actually going to kill two birds with one stone, since this code will also be used in the next section to plot the data.

To check that the data was recorded correctly, you can simply create a file named `readout.php`, which will read data from the database, format it, and print the formatted data so you that can see it. First, we need to access the SQLite database:

```
$db = new SQLite3('database.db');
```

We then need to make a query to the database so it returns the data we want. In this request, we are simply going to take all the fields from the table called measurements. This is done using the following piece of code:

```
$results = $db->query('SELECT id, timestamp, temperature, humidity FROM measurements');
```

Now, we need to use PHP to parse this variable that contains all the results from the query. We simply use a while statement for that:

```
while($row = $results->fetchArray())
{

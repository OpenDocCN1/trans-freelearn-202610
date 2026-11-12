
```
if (temperature > 23) {
    Serial.println("\nTriggered! Calling /Library/Google/Gmail/SendEmail...");

    // Send email
    runSendEmail(temperature);
}
```

You can see that every time the temperature goes above that limit, an alert will be sent by Temboo via e-mail. We will see the details of this function in a moment. I also inserted a 1-minute delay between each measurement, so we don’t continuously send e-mails if the threshold is crossed:

```
delay(60000);
```

Now let’s look at the details of the runSendEmail() function. This function is responsible for creating the correct Choreo to send an e-mail, set the body of the e-mail, and send it via Temboo.

It starts by creating the correct Choreo to send an e-mail with the following line:

```
TembooChoreoSendEmailChoreo(client);
```

We also define the different information relative to your account that is defined in the Temboo account file you just downloaded from Temboo:

```
SendEmailChoreo.setAccountName(TEMBOO_ACCOUNT);
SendEmailChoreo.setAppKeyName(TEMBOO_APP_KEY_NAME);
SendEmailChoreo.setAppKey(TEMBOO_APP_KEY);
```

Then, this is where you need to enter the name of the e-mail project that you just created on Temboo. If you use the same name as I did, there is no need to modify anything in the following line:

```
SendEmailChoreo.setSavedInputs("arduinoTemperatureEmail");
```

We will now build the body of the e-mail, which will be stored in a string variable. We simply use a generic message where we attach the value of the measured temperature and store everything in a string. Finally, we will add the following lines as an input to the Choreo used to send e-mails:

```
String MessageBodyValue = String("Alert! The current sensor value is: ") +
String(sensorValue);
SendEmailChoreo.addInput("MessageBody", MessageBodyValue);
```

Just before executing Choreo, we define that we want to use SendEmailChoreo from Gmail using the following line:

```
SendEmailChoreo.setChoreo("/Library/Google/Gmail/SendEmail");
```

We can finally execute Choreo and store the result in a variable:

```
unsigned int returnCode = SendEmailChoreo.run();
```

As in the previous section, if the result is 0, it means everything went fine. If this is not the case, we will print the error message on the Serial port for debugging using the following piece of code:

```
if (returnCode == 0) {
  Serial.println("Done!\n");
} else {
  // A non-zero return code means there was an error
  // Read and print the error message
  while (SendEmailChoreo.available()) {
    char c = SendEmailChoreo.read();
    Serial.print(c);
  }
  Serial.println();
}
```

Finally, we will close Choreo with the following line:

```
SendEmailChoreo.close();
```

## Note

You can find all the code for this section on the GitHub repository of this chapter at https://github.com/openhomeautomation/arduino-networking/tree/MASTER/chapter6.

You can now test the sketch. The first thing to do is to make sure that you have your own Temboo account file in the same folder as the Arduino sketch. Then, upload the sketch to the Arduino board and open the Serial Monitor. The first thing you should see is the confirmation message that everything went ok:

```
**DHCP: OK**
**Setup complete**
```

Then, you should see the first measurements on the Serial Monitor:

```
**Temperature: 21**
**Humidity: 39**
```

Depending on the temperature value, you might need to pinch your sensor to trigger the e-mail alert. Remember, in the example of this chapter, we set a value of 23 degrees for the temperature. After a while, if the temperature reaches the threshold, you should see the following message:

```
**Triggered! Calling /Library/Google/Gmail/SendEmail...**
```

After a while, the following confirmation message should appear:

```
**Done!**
```

You can now close the Serial Monitor and go check the e-mail account that you set on Temboo. You should have received a message with the title you set on Temboo, and you will also receive the body that indicates the generic message you set in the sketch along with the measured temperature.

## Summary

In this chapter, we created another Internet of Things project using the web service Temboo. Based on Temboo, we built two exciting applications. The first one was to automatically log measurements inside a Google Docs spreadsheet. Compared to the approach we took in the previous chapter, the advantage here is clearly that Google Docs is the widely used standard to create and share spreadsheets. You can access your data from anywhere just by using a web browser, and you can also plot the data and share it easily with your friends and family.

We also built an exciting application where we interfaced our project with Gmail via Temboo. We built an automated e-mail alert system based on the measured data.

Let’s see what the major takeaways of this chapter were. First, we built the hardware of the project, with the DHT11 temperature and humidity sensor on top of the Arduino Ethernet shield.

Then, we set up the different accounts that were needed for the project. We first created a spreadsheet on Google Docs and configured it correctly so it can work with Temboo. Then, we also created a Temboo account and configured it so that your Ethernet shield can send data to Temboo and log measurements to Google Docs.

After the setting up phase, we built the first application of this project, which consisted of logging data directly to a Google Docs spreadsheet. We also displayed this data using the Google Docs plotting capabilities.

Finally, we used Temboo again to create an automated e-mail alert system by sending an e-mail automatically to the address of your choice when a measurement reached a given threshold.

As this is the end of this book, let’s see what we learned about the Arduino Ethernet shield and what exciting applications we can create with it.

The first part of the book was all about running a web client on the Ethernet shield. We made a first test by just trying out the shield and seeing whether it could connect to the Internet and grab the contents of a page. Then, we built our first measurement system, where the Ethernet shield was connecting to a server running to your computer to log some data.

In the second part of the book, we gave more autonomy to the Arduino Ethernet shield by running a web server right on the Arduino board. We made the measurement project again, but this time by having a web server run on the board and logging data to a SD card inserted in the shield. We also connected a relay to the Arduino board and we controlled it via Ethernet.

Finally, we built two Internet of Things project using the platforms Xively and Temboo. In both these projects, we sent data to the Web using the Ethernet shield so that this data can be accessed and displayed graphically from anywhere in the world.

To conclude this book, I hope that all the information you found in the different chapters will give you the will to build even more exciting applications. The Arduino Ethernet shield is one of the best ways to build amazing connected applications with Arduino, and there is no limit to what you can do with the concepts you learned in this book.

## Index

### A

- account, Google
  - setting up / Setting up your Google Account
- account, Temboo
  - creating / Creating your Temboo account
- application, building with Arduino board
  - hardware requisites / Hardware and software requirements
  - software requisites / Hardware and software requirements
  - hardware configuration / Hardware configuration
- application, building with Arduino Ethernet shield
  - hardware requisites / Hardware and software requirements
  - software requisites / Hardware and software requirements
  - hardware configuration / Hardware configuration
- Arduino board
  - web server, creating on / Creating a web server on Arduino
- Arduino Ethernet Shield
  - URL / Hardware and software requirements, Hardware and software requirements
- Arduino IDE
  - URL / Hardware and software requirements
- Arduino library
  - installing / Hardware and software requirements
- Arduino Temboo library
  - URL / Hardware and software requirements
  - installing / Hardware and software requirements
- Arduino Uno
  - URL / Hardware and software requirements, Hardware and software requirements
- automated e-mails
  - sending / Sending automated e-mails

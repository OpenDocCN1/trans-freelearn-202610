Account credentials you have set before. If you used the same name as I did for this example, you don't have to modify anything in the following line:

```
AppendRowChoreo.setSavedInputs("arduinoTemperatureSpreadsheet");
```

Note that the new way to overcome the Temboo call limitation is also to transmit several data points at each call of the Temboo API, for example, by appending several rows at each call. You can modify the code accordingly as an exercise.

Now, we need to format the data so it can be recorded correctly in Google Docs. What you want to insert in a given row is a timestamp, the temperature, and the humidity. For the timestamp, we are simply going to use the time that has passed since the device was powered on with the millis() function.

The data itself will be contained inside a string variable, where the value of each column will be separated with a comma, as shown in the following code:

```
String RowDataValue = String(millis()) + "," + String(temperature) + "," + String(humidity);
AppendRowChoreo.addInput("RowData", RowDataValue);
```

We also need to set which Choreo we want to use with the following line:

```
AppendRowChoreo.setChoreo("/Library/Google/Spreadsheets/AppendRow");
```

Then, we can finally execute Choreo and store the result in a variable called returnCode using the following line:

```
unsigned int returnCode = AppendRowChoreo.run();
```

If this code is equal to 0 after Choreo has been executed, it means that everything went fine, and we will print the corresponding message on the Serial Monitor for debugging purposes using the following piece of code:

```
if (returnCode == 0) {
    Serial.println("Done!\\n");
} else {
    // A non-zero return code means there was an error
    // Read and print the error message
    while (AppendRowChoreo.available()) {
        char c = AppendRowChoreo.read();
        Serial.print(c);
    }
    Serial.println();
}
```

If that's not the case, we will print the corresponding error message on the Serial Monitor. Finally, we close Choreo with the following line:

```
AppendRowChoreo.close();
```

# Note

You can find all the code for this section on the GitHub repository of this chapter at https://github.com/openhomeautomation/arduino-networking/tree/master/chapter6.
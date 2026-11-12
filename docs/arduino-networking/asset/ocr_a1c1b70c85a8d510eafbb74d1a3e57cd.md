![](bbox=[0.058, 0.043, 0.665, 0.126])

After that, you will have to select the device you will be working with to log data to the spreadsheet. This is where you select the credentials that we created before, as shown in the following screenshot:

![](bbox=[0.06, 0.243, 0.785, 0.322])

You will also need to give a name to the project you just created so your Arduino sketch will later on know where to send the data. I named mine
arduinoTemperatureSpreadsheet. If you decide to use another name, you will have to
make a small modification in the code later.

When this is done, you will be prompted to download a compressed file that contains two
Arduino code files. Download it, unzip it, and have a look at it. It will appear like the
following screenshot:

![](bbox=[0.06, 0.53, 0.905, 0.59])

There should be an Arduino file and another file called TembooAccount .h. This last file is
the most important as it contains your credentials for the project. You will need to use this
file for the rest of the project in place of the file that is included in the GitHub repository
of the project.
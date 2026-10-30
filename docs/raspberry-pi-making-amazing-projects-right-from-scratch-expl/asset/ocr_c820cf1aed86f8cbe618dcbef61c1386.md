Open an editor of your choice, write the following code, and save it as **timelapse.sh**:

```
```bash
#!/bin/bash

DATE=$(date +"%Y-%m-%d_%H%M")
fswebcam -r 1280x960 --no-banner
/home/pi/book/output/timelapse/garden_$DATE.jpg
```
```

Make the script executable using:

```
```bash
chmod +x timelapse.sh
```
```

This shell script captures the image and saves it with the current timestamp in its name. Thus, we get an image with a new filename every time as the file contains the timestamp. The second line in the script creates the timestamp that we're using in the filename. Run this script manually once, and make sure that the image is saved in the **/home/pi/book/output/timelapse** directory with the **garden_<timestamp>.jpg** name.

To run this script at regular intervals, we need to schedule it in **crontab**.

The **crontab** entry to run our script every minute is as follows:

```
```bash
* * * * * /home/pi/book/chapter04/timelapse.sh 2>&1
```
```

Open the **crontab** of the Pi user with **crontab -e**. It will open crontab with **nano** as the editor. Add the preceding line to crontab, save it, and exit it.

Once you exit **crontab**, it will show the following message:

```
```text
no crontab for pi - using an empty one
crontab: installing new crontab
```
```

Our timelapse webcam setup is now live. If you want to change the image capture frequency, then you have to change the crontab settings. To set it every 5 minutes, change it to ***/5 * * * ***. To set it for every 2 hours, use **0 */2 * * ***. Make sure that your MicroSD card has enough free space to store all the images for the time duration for which you need to keep your timelapse setup.
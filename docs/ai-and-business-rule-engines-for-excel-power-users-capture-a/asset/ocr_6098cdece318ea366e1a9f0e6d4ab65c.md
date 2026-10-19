7. Check that we are in the right place (and have the right filename) by typing the dir command in the console, followed by Enter. You should see something close to Figure 10.25:

![](bbox=[0.275, 0.138, 0.74, 0.25])

Figure 10.25 – Making sure our Terminal is open in the right place

8. Make sure the spelling of our .yml file is exactly the same, all lowercase: docker-compose.yml.

9. Now that we are sure that we are in the right directory, type the following command to run, using the settings in this Docker Compose file: docker-compose up (and press Enter).

10. You may get a message asking you to log in first. If you get this, type docker login, then give your username and password when requested. Repeat step 9 once you have logged in.

11. Like when we ran our previous images, Docker checks for a local copy and will then fetch the KIE Server image if needed. This could take a couple of minutes to download for the first time. You’ll see the progress messages in the console as it does so.

12. You may get a message that the names of the image conflict with the name of an image already on your computer. If this happens, open Docker Desktop, delete the previous copies of the image and container, then try docker-compose up again.

13. When the number of messages in the console has slowed down, it’s worth opening Docker Desktop to check everything has started correctly. You should see the screen shown in Figure 10.26:

![](bbox=[0.145, 0.565, 0.87, 0.71])

Figure 10.26 – Docker Desktop showing both images we started with Docker Compose

14. Check that both containers are running correctly – in Figure 10.26, both are green (running). We can see Business Central running like before and our new KIE Server image running alongside it.
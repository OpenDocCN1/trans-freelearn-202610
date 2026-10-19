6. Power Automate will create our HTTP action. In Figure 8.7, we’ve already entered the settings for this action – not surprisingly, they are the same as we’ve used in Chapter 6 – a GET method and a URI of https://api.github.com/repos/OfficeDev/office-js. The other values we can leave blank.

![](bbox=[0.15, 0.19, 0.87, 0.45])

Figure 8.7 – Second step in the flow – HTTP Connector

7. Even though our flow is now longer, there will still be a New step option at the bottom. Click on it, then search for the Create file option from OneDrive. We won’t show the search process this time; if you have the right action, it will have an icon and appearance similar to Figure 8.8:

![](bbox=[0.3, 0.57, 0.72, 0.74])

Figure 8.8 – Settings for creating our file

8. In this action, we select the folder path – clicking on the Folder Path field will let you choose a path that already exists in OneDrive. Note, if you’ve just created a new username for Power Automate, you may need to sign in to OneDrive with this username and password first. We chose a general Downloads folder, but any existing folder will do.
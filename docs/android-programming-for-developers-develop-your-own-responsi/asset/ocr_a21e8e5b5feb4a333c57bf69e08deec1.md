# Preparing to connect to Google Maps

There are two main stages to get started. First we will see how to get an API key that our app will use to communicate with the servers and second we must make the app.

First, let's get an API key.

# Acquiring an API key

To get an API key, you first need a Google account. If you use Gmail or Google+, then you already have one. If not, you can get one for free at https://accounts.google.com.

In the values folder of the project explorer, find the google_maps_api.xml file that Android Studio created for you. It might already be open in the editor.

In this file, there is a link to the Google Developers Console website where we obtain our API key. Android Studio has been very helpful as it generated this link. More than this, within the link, all the details about our app that we would otherwise have to search for and that are required to obtain the key, are encoded. The location of the link within the file is highlighted in the following screenshot. I have obscured a few details as they are specific to my account. Yours will be in exactly the same place, but with different specific values:

![](bbox=[0.118, 0.61, 0.882, 0.75])

Copy the link from your google_maps_api.xml file identified in the previous screenshot and paste it into your web browser. You will see this next screen then. Agree with the terms of service and click on **Agree and continue**:
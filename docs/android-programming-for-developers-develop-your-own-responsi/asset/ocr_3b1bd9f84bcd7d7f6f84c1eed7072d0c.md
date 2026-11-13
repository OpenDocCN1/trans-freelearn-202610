# Project setup

We are going to use Android Studio as the IDE. We are going to create the project with minSDK 15 (Ice Cream Sandwich—ICS). As a good practice, we don't want to move the minimum SDK, unless we are using some features that were not available before. By keeping the minSDK low, you make your game available to as many devices as possible.

The two main features we are going to use from ICS are Fragments, ValueAnimators, and ViewPropertyAnimators. All of these were already available in Honeycomb, but 3.x is considered little more than a test for ICS; it was not mature and has been replaced by ICS in almost all devices.

In the unlikely case that you want to support older versions such as Gingerbread, you can make use of the compatibility library and NineOldAndroids to add backwards-compatibility for the features we are using.

# Creating the stub project

Let's go on and navigate to File > New Project. We are going to use YASS as the Application name and example.com as the Company Domain.
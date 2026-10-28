
4. Click App and click Continue:

Figure 27.13: Identifier type screen

5. Enter a description for this App ID, such as JRNL Packt Publishing App ID. Check the Explicit button and enter your app’s bundle ID in the field. Make sure that this value is the same as the bundle identifier you used when you created the project. Click the Continue button when you’re done:

Figure 27.14: Description and bundle ID screen

Once your app has shipped, you will no longer be able to change the bundle ID for your app.

6. Click Register:

Figure 27.15: Register screen

Your App ID has now been registered. Cool! In the next section, you’ll register the devices you’ll be testing your app on.

## Register your devices

To run your apps on your personal devices for testing, you will need to register them in your developer account. Here’s how to register your devices:

1. Log in to your Apple Developer account and click Devices.
2. Click the + button:

![](img/0972ba626734a3986e095c788777818e_582_0.png)

Figure 27.16: Device registration screen

3. The Register a New Device screen appears:

![](img/0972ba626734a3986e095c788777818e_582_1.png)

Figure 27.17: Register a New Device screen

You’ll need to provide the device name and device ID to register your device.

4. Connect your device to your Mac. Launch Xcode and choose Devices and Simulators from the Window menu. Choose the device in the left pane and copy the Identifier value:

Figure 27.18: Devices and Simulators window

5. Type a name for the device in the Device Name field and paste the identifier value into the Device ID (UDID) field. Click Continue:

Figure 27.19: The Register a Device screen

You have successfully registered your test devices. Great! The next step is to create provisioning profiles. An iOS App Development profile is required so that your apps will be allowed to run on your test devices, and an iOS App Store Distribution profile is required for apps that will be uploaded to the App Store. You will create development and distribution profiles in the next section.

## Create provisioning profiles

You will need to create two provisioning profiles. An iOS app development profile is required for apps to run on test devices. An iOS App Store distribution profile is used to submit your app to the App Store. Here’s how to create the development profiles:

1. Log in to your Apple Developer account and click Profiles.
2. Click the + button:

Figure 27.20: Profiles screen

3. Click the iOS App Development radio button and click Continue:

Figure 27.21: Register a New Provisioning Profile screen

4. Select the App ID for the app you want to test and click Continue:

Figure 27.22: Selecting an App ID screen

5. Select a development certificate checkbox and click Continue:

Figure 27.23: Selecting a development certificate

To test your app on your test devices, you’ll use the development certificate, App ID, registered test devices, and development profile. To submit your app to the App Store, you’ll use the distribution certificate, App ID, and distribution profile. You’ll configure Xcode to manage this automatically for you.

Before you submit your app, you must create your app’s icons and get screenshots of your app. Then, you can create an App Store listing, generate an archive build to be uploaded, and complete the App Store Connect information. Apple will then review your app and, if all goes well, it will appear on the App Store.

> To see more information on how to submit your apps, visit https://developer.apple.com/app-store/submitting/.

In the next section, let’s see how to create icons for your app, which will appear on a device’s screen when the app is installed.

## Create icons for your app

Before you upload your app to the App Store, you must create an icon set for it. Here’s how to create an icon set for your app:

1. Create an icon for your app that is 1,024 x 1,024 pixels.
2. Click the Assets file in the Project navigator and drag the icon you created to the space shown in the following screenshot:

![](img/0972ba626734a3986e095c788777818e_588_0.png)

Figure 27.32: Assets file showing space for the app icon

When you run your app in Simulator or on a device and quit your app, you should be able to see the app’s icon on the home screen. Neat!

As shown in Chapter 24, Icon Composer, you can also use Icon Composer to create an icon for your app.

Let’s look at how to create screenshots next. You’ll need them for your App Store submission, so customers can see what your app looks like. You’ll do this in the next section.

## Take screenshots for your app

You’ll need screenshots of your app, which will be used in your App Store listing. To create them, run your app in Simulator and click the screenshot button. It will be saved to the desktop:

![](img/0972ba626734a3986e095c788777818e_589_0.png)

Figure 27.33: Simulator showing the screenshot button

Apple requires screenshots of your app running on an iPhone with a 6.7” display (such as the iPhone 14 Plus) and an iPad with a 13” display (such as the iPad Pro 13-inch). You can use Simulator to simulate these devices if you don’t have the actual devices, and your screenshots should show your app’s features and how your app looks on different screen sizes.

How to submit screenshots of your app will be discussed in more detail in the next section, where you will learn how to create an App Store listing. The App Store listing contains all the information about your app that will be displayed in the App Store, so customers can make an informed decision about downloading or purchasing your app.

## Create an App Store listing

Now that you have icons and screenshots of your app, you’ll create the App Store listing. This allows customers to see information about your app before they download it. Follow these steps:

1. Go to http://appstoreconnect.apple.com and select Apps:

![](img/0972ba626734a3986e095c788777818e_590_0.png)

Figure 27.34: The App Store Connect website

2. Click the + button at the top left of the screen and select New App:

![](img/0972ba626734a3986e095c788777818e_590_1.png)

Figure 27.35: The New App button and menu

3. A New App screen displaying a list of fields will appear:

![](img/0972ba626734a3986e095c788777818e_591_0.png)

Figure 27.36: App details screen

4. Enter your app details:

- Platforms: All the platforms your app supports (iOS, macOS, and/or tvOS).
- Name: The name of your app.
- Primary Language: The language your app uses.
- Bundle ID: The bundle ID you created earlier.

3. Select **App Information** on the left side of the screen, and make sure all the information is correct:

![](img/0972ba626734a3986e095c788777818e_597_0.png)

Figure 27.46: The App Information screen

4. Do the same for the **Pricing and Availability** and **App Privacy** sections:

![](img/0972ba626734a3986e095c788777818e_597_1.png)

Figure 27.47: The App Privacy screen

5. Select Prepare for Submission on the left side of the screen. In the App Preview and Screenshots section, drag in the screenshots that you took earlier:

![](img/0972ba626734a3986e095c788777818e_598_0.png)

Figure 27.48: Prepare for Submission screen showing App Preview and Screenshots section

6. Scroll down and fill in the Promotional Text, Description, Keywords, Support URL (containing support information for your app), and Marketing URL (containing marketing information for your app) fields:

| Field | Value |
| --- | --- |
| Promotional Text | Simple journal to keep track of important moments in your life. |
| Description | Simple journal to keep track of important moments in your life. |
| Keywords | #Journal |
| Support URL | https://tomafuwi.tumblr.com |
| Marketing URL | https://tomafuwi.tumblr.com |

Figure 27.49: Version information section

7. Scroll to the general app information section and fill in all the required details:

| Field | Value |
| --- | --- |
| Version | 1.5 |
| Copyright | Tomafuwi Productions Sdn. Bhd. |
| Routing App Coverage File | Choose File (Optional) |

Figure 27.50: General app information section

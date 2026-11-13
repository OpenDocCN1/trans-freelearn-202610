# # Project configuration

There are two different approaches to adding Android TV to your project. They are conceptually different but the code you have to write is almost the same for both of them, although in different places. According to the characteristics of your project, you may prefer one or another. These options are:

+   - Building a separate APK for Android TV
+   - Using the same APK for phones, tablets, and TVs

The default option of Android Studio is to create a build variant for Android TV. It did that when we used the wizard to set up our project and we added TV support. That is why we have the mobile and tv directories. The main project is mobile and tv is configured to be a build variant or "flavor".

Flavors are a powerful way to build different APKs from the same code base, allowing us to modify parts, be it code or resources. It is particularly useful when you have to publish the same game in different markets. On the other hand, if you have a single APK, it has to be able to adapt to the different configurations on its own.

Each approach has advantages and disadvantages.

Some reasons why you would want to have a separate APK for Android TV are:

+   - Using a lower target API for the mobile version (Android TV requires target SDK 21 – Lollipop)
+   - Using higher-resolution assets for TV without including them into the mobile package
+   - Having separated special features for mobile/TV

On the other hand, the main advantages of having a single APK are:

+   - Easier to publish (only one file to upload)
+   - Easier to maintain

Given that we will not have any special features built in for Android TV, we are going to use the single-APK approach. Note that, if you want to do a build variant, the modifications are almost the same, but done in the tv directory
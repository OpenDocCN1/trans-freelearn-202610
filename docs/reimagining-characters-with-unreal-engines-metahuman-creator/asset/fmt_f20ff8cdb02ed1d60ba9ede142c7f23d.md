
This is what the screen will look like:

![Figure 1.10: Virtual Production template](image/B17878_01.10.jpg)

*Figure 1.10: Virtual Production template*

> **Note**
>
> If you happen to have a lower specification machine that doesn’t support raytracing, it’s wise to keep `Raytracing` to the default off mode. You can always enable it again in `Project Settings` at a later stage.

From this list, we’re just going to focus on the `Virtual Production` subtemplate. Then, we need to choose our new project location and create a project title. For my project, I’m going to create a folder titled `Lost Girl` and it will be in the `D drive`. The project folder will be `Lost_Girl_SC_01`. You can name your project file whatever you like.

Feel free to save your files to whatever directory you wish but be advised that it is best practice to avoid the `C drive` where your operating system is stored.

You’ll see a splash screen again and you will be prompted that the project is loading. This will take a minute or two depending on your machine’s specifications. After this, you may get a number of popups:

*   You may also get a dialog box just as earlier warning you that a plugin isn’t designed to work in UE5 and asking whether you still want to proceed. Choose `Yes` if that happens again.
*   You also may get asked whether you want to update the project. Choose `Yes`. This will just take a second or two to update.
*   Finally, it will ask whether you want to manage plugins. You can choose `Yes` and then close down the plugin window if that happens. You can also choose `No` and enable the plugins later as you import your character into the engine, but I do recommend selecting `Yes` at this point.

That’s it! We now have the engine up and running.

![Figure 1.11: The Unreal Engine interface](image/B17878_01.11.jpg)

*Figure 1.11: The Unreal Engine interface*

Next, it’s time to install Quixel Bridge.

## Installing Quixel Bridge

MetaHuman Creator is an online application from Quixel. To get that up and running, first, we need to go to their website and download the application called Bridge that allows us to launch the MetaHuman Creator online application.

This sounds a little complicated, but the Bridge application works like an asset management tool with your account details. For example, it will fire up your personal MetaHumans projects and allow you to download many other assets, such as Megascans Trees, grass, buildings, and so on while keeping tabs on everything you’ve created or downloaded in the past.

This can come in incredibly handy because MetaHumans take up a lot of hard drive space, so you get to store your creations in the cloud, which allows you to download them wherever you are. In addition, once installed, Bridge also works as a plugin within Unreal Engine, making asset management even easier.

So, to get started, complete the following steps:

1.  First, go to the Quixel website: `www.quixel.com`.
2.  Next, click on the `DOWNLOAD BRIDGE` button (we don’t even need to log in at this stage):

![Figure 1.12: Downloading Quixel Bridge](image/B17878_01.12.jpg)

*Figure 1.12: Downloading Quixel Bridge*

3.  Once we have downloaded the installer, `Bridge.exe`, run it by double-clicking the file.

This will install and open Bridge automatically and should pop up as a small window like so:

![Figure 1.13: Quixel Bridge pop-up application](image/B17878_01.13.jpg)

*Figure 1.13: Quixel Bridge pop-up application*

4.  Click on the `SIGN IN` button in the top-right corner of the page.
5.  This will lead to another dialog box where we can choose `SIGN IN WITH EPIC GAMES`.

`SIGN IN` at the top right will change to a person icon, indicating that you have successfully signed in:

![Figure 1.14: Successfully signed in](image/B17878_01.14.jpg)

*Figure 1.14: Successfully signed in*

We’re nearly all set up. Now, let’s finally set up MetaHuman Creator.

## Booting up MetaHuman Creator

Now that you’ve downloaded Bridge onto your computer and signed in using the same login credentials you used for the Epic Games Launcher, you are now ready to boot up MetaHuman Creator. The difficult part is over, and this is where the fun begins.

It is worth noting at this point that Quixel’s Bridge has more features than just MetaHumans. Bridge gives you access to an enormous library of 3D assets, such as Megascans, and 2D assets, such as textures and materials. Unlike most 3D asset libraries, Bridge provides scanned models. In other words, these models are based on data taken from real objects, which is why they look so realistic but come with a hefty file size.

To get straight into MetaHumans, look for the person icon in a circle on the left-hand side of the screen:

![Figure 1.15: The MetaHumans icon](image/B17878_01.15.jpg)

*Figure 1.15: The MetaHumans icon*

You will see numerous thumbnails of MetaHuman templates. By clicking on any one of these thumbnails, you will see a new option related to that character you’ve clicked, and you’ll see a blue button titled `START MHC` (this stands for MetaHuman Creator):

![Figure 1.16: Starting MHC](image/B17878_01.16.jpg)

*Figure 1.16: Starting MHC*

And that’s it. Once you click the blue button, you will start your MHC session. This will take a few moments to boot up and you’ll get a page looking something like this:

![Figure 1.17: Connecting to the MetaHuman Creator online session](image/B17878_01.17.jpg)

*Figure 1.17: Connecting to the MetaHuman Creator online session*

As mentioned earlier in this chapter, be sure to have a reliable internet connection to speed up the process of MHC.

# Summary

In this chapter, you’ve learned what Unreal is, what MetaHumans are, and what software applications you need to create MetaHumans.

The next chapter is entirely about the MetaHuman Creator online session. While there are a lot of features, you will find that the application is very intuitive and, in many ways, what you have learned in this chapter is much more complicated than what’s to come. In fact, you’re going to find the next chapter quite enjoyable!

# Creating Characters in the MetaHuman Interface

In the previous chapter, you learned how to install the Epic Games Launcher, the UE5 engine, and Bridge to your machine. Additionally, you went through the steps of setting up an Epic Games user account, which you must use again to sign into Quixel Bridge.

This chapter will cover everything you need to know about the `MetaHumans Creator (MHC)` and the online session where all the character designing takes place. We will look at all the editable features of the many MHC templates, including the face, hair, body, blend, move, and sculpt parameters, as well as the best practices of when to use each attribute within the character design phase.

The primary goal of this chapter is to get you accustomed to the interface and the character design potential, but it will also provide notes on how to implement the same design tools for a more acutely focused design process.

In addition to the various tools and preset editors, you will become familiar with learning just how MHC has become the best-in-class of photorealistic character design as we delve into some of the science around skin shaders.

So, we will cover the following topics:

*   Starting up Quixel Bridge
*   Editing your MetaHuman
*   Using the `Move and Sculpt tools`
*   Downloading and exporting your character

## Technical requirements

We will be continuing from the previous chapter, so you will need UE5 and Bridge installed, along with a reliable internet connection. A strong internet connection is particularly important here as you will be downloading over 1 GB of data per character.

## Starting up Quixel Bridge

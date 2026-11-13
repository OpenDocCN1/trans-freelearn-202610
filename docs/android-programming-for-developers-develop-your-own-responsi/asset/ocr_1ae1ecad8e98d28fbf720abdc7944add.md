# Our first working Fragment mini app

Let's build a Fragment in its simplest possible form so that we can understand what is going on before in later chapters we start producing Fragments all over the place.

# Tip

I urge all readers to go through and actually build this project. There is a lot of jumping around from file to file and just reading alone can make it seem more complex than it really is. You can copy and paste the code from the download bundle but please also follow the steps and create your own projects and classes. Fragments are not too tough but their implementation, like there name suggests is a little fragmented.

Create a new project called Simple Fragment using the same default settings as always. Note that there is the option to create a project with Fragment, but you will learn more about them by doing things from scratch.

Switch to activity_main.xml and delete the default Hello world! widget. Now, make sure that the RelativeLayout root is selected by clicking on it in the Component Tree window. Change its id property to fragmentHolder. We will now be able to get a reference to this layout in our Java code, and as the id property implies, we will be adding a Fragment to it.

Now, we will create a layout that will define our Fragment's appearance. Right-click on the layout folder and navigate to New | Layout resource file. In the File name field, type fragment_layout and click on OK.

Add a single Button widget anywhere on the layout and make its id property button.

Now that we have a simple layout for our fragment to use, let's write some Java code to make the actual fragment.

# Tip

Note that you can create a Fragment by simply dragging and dropping one from
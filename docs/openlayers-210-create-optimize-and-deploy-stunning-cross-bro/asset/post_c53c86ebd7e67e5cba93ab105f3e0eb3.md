
```
function update_vector_layer() {
    //Change URL based on input tags
    vector_layer.protocol.options.params['tags'] = document.getElementById('input_tags').value

    //Refresh the layer with the new params
    vector_layer.refresh();

    //Lastly, clear out the div that shows photo info
    document.getElementById('photo_info_wrapper').innerHTML = '';
}
```

3. Lastly, we'll need to make the input_submit button call the function when it is clicked. Now, we're going to be using JavaScript's built in event handling— but it's quite similar to what we do in OpenLayers. We just call the addEventListener method of an element. To get that element, we'll use document.getElementById like we've been doing throughout the book. When calling it, we pass in the event type ('click' in this case), the name of a function to call, and a Boolean that specifies how the event propagates (if you're not sure about this, just leave it as false).

```
document.getElementById('input_submit').addEventListener(
    'click',
    update_vector_layer,
    false);
```

4. Open up the page and you should see an input box next to the map. When you change the value and hit the button next to it, the update_vector_layer function will get called and refresh the vector layer. Try typing in various tags (or multiple tags separated by a comma) and hitting the button:

![](img/1b8cd151c9f278555f9cf255b20d244e_347_0.png)

## What Just Happened?

We just updated our Flickr application to allow user input that affects what data is shown. We did this in a sort of modular way—we didn’t have to change any of the previous code we wrote. Instead, we just updated the data source and added additional functionality. Now that we’ve written an application, let’s talk a little bit about how to deploy it.

## Deploying an application

What does it mean to **deploy** an OpenLayers (or any other) application? Basically, deploying something means that we're switching from a development mode to a production mode; we're releasing something for the rest of the world to see.

The production application should be as fast and bug free as possible. Because we want the production version to be accessed as quickly as possible, this will often include removing things we used in the development environment and tweaking the production environment to better handle a lot of users. It also means using files that are as small in size as possible. One of the first things we should do to deploy our application is to use the OpenLayers build script to create a library file which is much smaller in file size.

![](img/1b8cd151c9f278555f9cf255b20d244e_348_0.png)

> There are many other things we can do to better prepare our production environment, such as using caching as well as combining and minimizing our JavaScript files. These, and other practices, are outside the scope of this book, but further information can be found on the book's website at http://vasir.net/.

## Building the OpenLayers Library file

OpenLayers provides an easy way to configure the library file that gets included in your page. Throughout the book, we've been including a file called `OpenLayers.js`, which is a file that contains *all* the functionality (classes, functions, and so on) that OpenLayers provides. So far, this has been great—we've been developing up to this point, so we want to be sure that we have access to all the classes that we may use. What's my point here?

## Always try to serve small files

Well, the `OpenLayers.js` file we've been using is around 920 KB! That's almost a megabyte—quite a large file to load, especially since it contains only JavaScript code. Even though large file sizes aren't as much of an issue as they were years ago, when everyone was on slower connections, a one megabyte JavaScript file in a production environment is something we want to avoid if at all possible.

We want all our files in a production environment to be as small as they can absolutely be. This will allow users to download the files faster (since there is less to download) which decreases the page's loading time (saves on bandwidth expenses). Faster page loads (even if the speed is only perceived) will greatly enhance the user's experience. Fortunately, we can greatly reduce the size of the OpenLayers library file with a build tool provided by OpenLayers.

## Using the OpenLayers build file

When you download the OpenLayers archive file, there is a folder inside it called as `build`. Inside that, there is a file called `build.py`, which is a python script that creates an `OpenLayers.js` file based on a separate build configuration file. Building the library this way is called a **single file build**, as all the JavaScript code behind OpenLayers is contained in a single file.

So, what we can do is tell the build script to create an `OpenLayers.js` file that includes *only* the classes our application needs. This means that we can't access other classes, but in a production environment we won't need to, as the code won't be changing—that's what development is for. The benefit to doing this is because if the file has less code in it, the file size will be smaller.

## Configuring the build script

When the build script runs, it looks for a configuration file to determine which classes to include and exclude when creating the `OpenLayers.js` file. There are two configuration files OpenLayers ships with by default:

- **full.cfg:** Causes the build script to include everything
- **lite.cfg:** Includes just a minimal amount of classes to create a WMS or tiled layer, and does not include controls

You can create your own files by copying one of these files and specifying the files you want to include. When creating your own build configuration file, you must include at least the section labeled `[first]` from the `full.cfg` or `lite.cfg` files.

The `[include]` section specifies what files should be added to the `OpenLayers.js` output file. If left blank, *all* files will be included. Otherwise, only files specified in this section get included. To find out what files should be included here, look at your code and whenever you see a reference to the class (e.g., when calling `new OpenLayers.Classname`), it means you'll need to include that class here.

> The build script will automatically look for class dependencies, so you do not need to specify a parent class. For example, if you wish to include an individual control class, you can specify that class name and the build script will automatically include the base `Control.js` file.

The `[exclude]` section will be read in if the `[include]` section is left blank. These are classes that will *not* be included in the final build file. However, if you specify classes in the `[include]` section, this section will be ignored and only the classes in the `[include]` section will get included.

## Time for Action – building a Config file

Let's take a look at how to write a build config file.

- 1. Copy the `lite.cfg` file found in the build directory and name the copied version `example_1.cfg` (or whatever you'd like, but that's what we'll refer to it as).
- 2. Let's manually specify some files. We'll add in a couple controls. Add in the following and then save the file. The full file should look like this:

```
# Config File
[first]
OpenLayers/SingleFile.js
OpenLayers.js
OpenLayers/BaseTypes.js
OpenLayers/BaseTypes/Class.js
OpenLayers/Util.js
[last]
[include]
OpenLayers/Map.js
OpenLayers/Layer/WMS.js
OpenLayers/Control/LayerSwitcher.js
OpenLayers/Control/Navigation.js
OpenLayers/Control/Scale.js
[exclude]
```

## What Just Happened?

We just created a configuration file for our build named `example_1.cfg`. We'll be using this file to build the `OpenLayers.js` file. Notice that we did not have to include the base `Control.js` class—the build script will automatically look for and include the dependencies.

## Pop Quiz – using the Build script

Now that you know how to use the build script, can you come up with some reasons why you think using it would be a good idea? What are some of the benefits of using it? What would be a downside of using it?

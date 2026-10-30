
## Creating a simple photo gallery using a blank composition

We'll create a photo gallery consisting of three images. With a blank composition, we "tell" Muse which trigger matches up to the large image that will appear in the target area. Perform the following steps to create a simple photo gallery:

- 1. Open your page in the Design view and drag out the Blank composition widget from the Widget Library menu.
- 2. Click on the Composition widget once to select the entire widget. You can now position it anywhere you want on the page.
- 3. Click on the first light-gray trigger thumbnail under the target area to select it.
- 4. Click on the Fill link on the Control bar. From the drop-down menu, click on the Image icon and browse to choose the first Windsurfing image in the Composite Widgets Images panel. Set the Fitting to Scale To Fill, as shown in the following screenshot:

![](img/8fa9b5d33ed7ba830d1ac29ef24477e4_204_0.png)

Notice that the thumbnails in the list of states in the States widget are now all the same. If you want to create a rollover effect, you could click on the Rollover state in this widget and then change the image as we did previously. For this example, we are keeping everything nice and simple, so we'll use the same thumbnail for all four available states.

- 5. Add two more thumbnail images to the remaining triggers by repeating step 4. Choose the images from the Composition Widgets Image folder.
- 6. If you want to add additional thumbnails, click the plus (+) icon that appears to the right of the trigger elements. If you want to remove a trigger, select it and press Delete on your computer's keyboard.

## Add content to a target area

Our next step is to add the larger images to the target area using the following steps so that when we click on the thumbnail the correct image appears:

- 1. In the Design view, click on the Composition widget once again to select the entire widget.
- 2. Click once on the first thumbnail to select the trigger.
- 3. Click on the larger content area to select it. By clicking first on the trigger and then on the target, we are telling Muse that these two are linked.
- 4. With the content area selected, click on the drop-down Fill menu and add the first image. Once again, set Fitting to Scale To Fill.

> In this example, we're adding an image, but you can add any type of content that you like to the target.

5. Repeat steps 1 to 4 to add content to the target for the second and third thumbnails. When you've added the images to both trigger and target, it should look like the following screenshot:

![](img/8fa9b5d33ed7ba830d1ac29ef24477e4_206_0.png)

6. Test your photo gallery in the Preview view and make sure that your large image matches up with your thumbnail image.

> You can change the size of each thumbnail and each target area individually. Simply click once to select the entire widget, click once again to select either a trigger (thumbnail) or a target. Once the element is selected, a bounding box appears with handles in each corner. Dragging on the handles allows you to resize the element.

## Changing Composition widget options

Each Composition widget offers several options for setting up how the widget functions. To get to the options, click on the editing options icon—the blue arrow. In the following list, you'll find an explanation of each option available for the blank composition we've just added to our page:

- **Position**: This sets up where the target area is positioned on the page. By choosing **Stacked**, all of the content is overlapped, such as the target content appears in the same place on the web page. By choosing **Scattered**, it lets you position each target container in a different location on the page. Choosing **Lightbox** dims the rest of your web page and puts the focus on your target content.

- **Event**: We can choose to change the content in the target area when the user clicks on the thumbnail – **On Click** or when the user rolls their mouse over the thumbnail – **On Rollover**, as shown in the following screenshot:

![](img/8fa9b5d33ed7ba830d1ac29ef24477e4_207_0.png)

- **Transition**: We can specify how the content changes when the trigger is clicked or rolled over. The choices of animated transitions are:

  - **Fading**: This fades in the new target content while simultaneously fading out the previous target.
  - **Horizontal**: This slides the new target content in, with a horizontal sliding motion.
  - **Vertical**: This slides the new target content in, with a vertical sliding effect.
  - **Speed**: The choices here specify the speed of the transition. If you want an instant transition, select **None**.

- **Auto Play**: This option creates a slideshow requiring no user interaction.
- **Hide All Initially**: This option hides all target areas when the page is first loaded. In order to see the content, the user needs to click on the trigger.
- **Parts**: If you want to add extra user interactivity, you can use these options to show previous, next, or close buttons with the widget.
- **Show Lightbox Parts While Editing**: If you choose the **Lightbox** option under **Position**, you can then hide or display the contents of the target area while editing in the **Design** view.
- **Show All in Design Mode**: This option lets you display all target content areas at the same time when working in the **Design** view. By default this option is deselected, meaning only the target content of the selected trigger element appears in the **Design** view.

## Another way to create triggers and targets with multiple images

In the previous example, we manually added images first to the trigger and then to the target area. We set each trigger and target individually. There is another way to add multiple images simultaneously to the target area and Muse automatically creates the corresponding trigger elements for you using the following steps:

- 1. In the **Design** view, add a new blank **Composition** widget to the page.
- 2. Click on the **Composition** widget once to select the entire widget.
- 3. Click again to select the large target area.
- 4. Choose **File | Place**.
- 5. Select the multiple images you want to place. Hold down the *Ctrl* (Windows)/*Cmd* (Mac OS) key to select multiple images from the folder.
- 6. Click on **Select** (Windows) or **Open** (Mac OS).

7. Place the image group by clicking inside the target's content area as shown in the following screenshot. Muse automatically adds trigger elements for each image:

![](img/8fa9b5d33ed7ba830d1ac29ef24477e4_209_0.png)

8. Unfortunately, it doesn't add the corresponding images to the thumbnail trigger. If you want to do that, then select each trigger element and place images that match the corresponding target areas to create thumbnail image triggers.

## Slideshow widgets

The Slideshow widgets work in a very similar fashion to the Composition widgets, the main difference being the Slideshow widgets work exclusively with images while the Composition widgets can work with any type of content. The types of Slideshow widgets available in the Widgets Library menu are:

- **Basic**: The Basic Slideshow widget consists of a large content area, surrounded by a dark gray border. Underneath the content area there is a caption area, the number of slides or images which make up the slideshow, and a next and previous button. When you drag out the widget, it comes preloaded with three slides.

- **Blank**: The **Blank Slideshow** widget is similar to the **Basic Slideshow** widget except that it is empty when you drag-and-drop the widget out onto the page.
- **Lightbox**: The **Lightbox Slideshow** widget dims the rest of the page while the gallery element being displayed is active. This is a very popular type of slideshow on the web as it keeps the visitor's focus on the gallery element while reducing other distractions on the page.
- **Thumbnails**: The **Thumbnails Slideshow** widget shows thumbnails, which by default are positioned to the left of the larger content area. As the user clicks on the thumbnail, the image becomes bigger.

The following screenshot shows the different types of slideshows:

![](img/8fa9b5d33ed7ba830d1ac29ef24477e4_210_0.png)

Like the other widgets in Muse, the Slideshow widgets are completely editable and customizable.

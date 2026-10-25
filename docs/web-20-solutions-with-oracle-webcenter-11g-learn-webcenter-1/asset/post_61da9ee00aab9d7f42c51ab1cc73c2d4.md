

## 1. Execute JDeveloper, and in the Application Navigator choose Paracas.

![](img/fccf6c511126cf79e98510edd3b343ac_225_0.png)

## 2. Right-click on the ViewController project and choose New. Next, in the New Gallery window, select the category Web Tier | JSF, choose the option JSF Page, and click the OK button.

![](img/fccf6c511126cf79e98510edd3b343ac_225_1.png)

3. For the next step, check the **Create as XML Document** option, define the name customInformation.jspx for the page, choose the **Page template** option, select templateParacas, and click the OK button.

![](img/fccf6c511126cf79e98510edd3b343ac_226_0.png)

For pages that use Oracle Composer, components must be created with a .jspx extension.

4. Once the page is created, we will only use the region customRight. Therefore, in the component palette choose the category ADF Faces and drag the **Panel Box** control onto the region customRight in order to define a container.

![](img/fccf6c511126cf79e98510edd3b343ac_226_1.png)

## 5. Go to Property Inspector and change the Text property for Custom Information.

![](img/fccf6c511126cf79e98510edd3b343ac_227_0.png)

## 6. Next, we will include, in the panel box, a link that allows the user to activate the design mode on the page. Therefore, in the Component Palette, select the Oracle Composer category and drag the Change Mode Link.

![](img/fccf6c511126cf79e98510edd3b343ac_227_1.png)

## Personalization with Oracle Composer

7. Now it is time to define the region that can be customized by the user. This can be achieved by dragging the **Page Customizable** component to the area defined in the previous screenshot.

![](img/fccf6c511126cf79e98510edd3b343ac_228_0.png)

8. Go to the palette structure and notice that the Page Customizable component has been generated. Additionally, under this component a **panelCustomizable** container is generated.

![](img/fccf6c511126cf79e98510edd3b343ac_228_1.png)

9. The Panel Customizable container does not allow the user to divide the region in other ways. For that reason, we are removing that container and we are going to replace it with a Layout Customizable container which has that capability. So, first remove the container, **Customizable Panel** by right-clicking on the container and choosing the **Delete** option.

10. Now, drag the **Layout Customizable** component within the control Page Customizable, as shown in the following screenshot:

![](img/fccf6c511126cf79e98510edd3b343ac_229_1.png)

## 11. Test the page by right-clicking on the page and choosing the **Run** button.

![](img/fccf6c511126cf79e98510edd3b343ac_230_0.png)

Finally, it will display the next page. As you can see, we now have a basic page in runtime mode. When the user clicks on the edit link, the page will change to edit mode and so the user can customize its contents.

![](img/fccf6c511126cf79e98510edd3b343ac_230_1.png)

## Oracle Composer for end users

Once the environment has been prepared, it is time that the end user can customize their content.

Before performing the customization, it is important to know the following technologies, which are behind the Oracle Composer.

Oracle Metadata Service and Oracle Composer allow each user to customize the content of the pages. In this way, each user will have a different version of a base page.
You’ll probably wonder: how does this act on Oracle Composer? The answer is thanks to Oracle Metadata Service.
This technology allows us to store any user customizations in a repository based on XML. That is, at run time, any customizations made by users, do not change the application code but are stored in XML files associated with each user of the application.

## Resource catalog

The resource catalog has the components that end users can use in design mode to customize his content.
Generally, the tool provides a default set of components, but this can be configured to expose portlets, task flow, JSF components, and other types of components.

![](img/fccf6c511126cf79e98510edd3b343ac_231_0.png)

## Content customizing using Oracle Composer

Using the previously developed example, we are going to customize page content.
Notice that the page must be in runtime mode.

1. First, let's change the page to design mode. To do this, click the **Edit** link.

![](img/fccf6c511126cf79e98510edd3b343ac_232_0.png)

2. What is shown now is the design environment of the page. In this mode, the user can customize the content of the region that we have prepared in the first part of this chapter.

![](img/fccf6c511126cf79e98510edd3b343ac_232_1.png)

3. In our case, we will customize the content of the page for reporting the impact of using natural fibers in garment making. So, initially we are going to change the title of the work area. To do this, click the **Page Properties** button.

![](img/fccf6c511126cf79e98510edd3b343ac_233_0.png)

4. You’ll see a screen where you can change the title of our work area. Change the title to Natural Fibers - Cotton, Wool and press the OK button.

![](img/fccf6c511126cf79e98510edd3b343ac_233_1.png)

5. Now let’s divide the work region, by clicking the Change Layout button and choosing Two columns, narrow right, as shown in the following screenshot:

![](img/fccf6c511126cf79e98510edd3b343ac_233_2.png)

## Personalization with Oracle Composer

6. With the last change, two regions have been created. Now, we will populate the contents of the left region by clicking the **Add Content** button of that region.

![](img/fccf6c511126cf79e98510edd3b343ac_234_0.png)

7. The following window will display the catalog of components. Click on the category **ADF Faces Components** to display the available components.

![](img/fccf6c511126cf79e98510edd3b343ac_234_1.png)

8. From the list of components available, the HTML Markup component allows us to directly include HTML and JavaScript content on the page. To do this, click the **Add** button.

![](img/fccf6c511126cf79e98510edd3b343ac_234_2.png)

9. Finally click the **Close** button.

10. Note that a HTML Markup control was included in the page. Now we will change its properties; to do this, click on the **Edit** button.

![](img/fccf6c511126cf79e98510edd3b343ac_234_3.png)

11. Now, we will include here a reference to a YouTube video related to the content of our page. To do this, open another browser window and locate the following video: http://www.youtube.com/watch?v=KuwBcycCM0w

12. Click the **Embed** button to see the code that allows us to include this video on our site.

![](img/fccf6c511126cf79e98510edd3b343ac_235_0.png)

13. Copy the code for the video.

![](img/fccf6c511126cf79e98510edd3b343ac_235_1.png)

14. Return to the designed page and paste the copied code into the value field. After changing the value property, click the **OK** button.

![](img/fccf6c511126cf79e98510edd3b343ac_235_2.png)

15. The designed page will be shown as follows:

![](img/fccf6c511126cf79e98510edd3b343ac_236_0.png)

16. We now proceed to populate the right region. To do this, click on the **Add Content** button of this zone.

![](img/fccf6c511126cf79e98510edd3b343ac_236_1.png)

17. Choose the Image component that allows us to expose a picture on the page. To do this, click the **Add** button and close the Catalog Window.

![](img/fccf6c511126cf79e98510edd3b343ac_236_2.png)

18. With this last change, a picture is added on our website. Now, let's set that image. To do this, click on the **Edit** button.

![](img/fccf6c511126cf79e98510edd3b343ac_237_0.png)

19. Change the **Image Source** property to the following URL associated to a picture that shows the growth of natural fiber production.
`http://www.fao.org/fi/figis/graphs/species/Aquaculture/2965.gif`

![](img/fccf6c511126cf79e98510edd3b343ac_237_1.png)

20. Press the **OK** button to complete the change.
21. In order to write information about the meaning of the video and image, click the **Add Content** button in right zone.

![](img/fccf6c511126cf79e98510edd3b343ac_237_2.png)


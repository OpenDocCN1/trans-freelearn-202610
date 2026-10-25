

3. In the next page, we define the following information:

- **Name**: Identifies the name of the application. In our case, our application will be called Paracas.
- **Directory**: This defines the directory where we will store the code of our application. For the project, the directory will be `c:\Paracas`, so you need to create it beforehand. Henceforth, we will refer to that directory by `<app_directory>`.
- **Application Package Prefix**: We will use this property in order to identify the root package from which we build our applications. The current application has the `com.paracas` package as a root.

![](img/fccf6c511126cf79e98510edd3b343ac_121_1.png)

## Including a template for our application

Before starting the development of any page, it is necessary to create a template that has the colors and logos that identify our company. All the pages of our application will use this template in order to reduce the design time for each page. The steps for this activity are as follows:

1. Go to the Application Palette, right-click on the project ViewController, and choose New.

![](img/fccf6c511126cf79e98510edd3b343ac_122_0.png)

2. In the window New Gallery choose the JSF category, and the JSF Page Template option, as shown in the following screenshot, and click the OK button.

![](img/fccf6c511126cf79e98510edd3b343ac_122_1.png)

3. In the next window, we need to enter the following data:
   - **File Name**: templateParacas.jspx
   - **Directory**: C:\Paracas\ViewController\public_html\templates
   - **Page Template Name**: templateParacas

Do not press the OK button, yet.

4. Our template will have three regions, one located at the top and two regions located to the left and right. The upper region will be fixed, and it will have the logo of our company. The following two regions will be variable, that is, the content of these will alter in the pages that are developed based on this template’s variable regions. The next figure shows the layout of the regions in the template:

![](img/fccf6c511126cf79e98510edd3b343ac_123_0.png)

5. During the creation of the template, we need identify the regions that are variable. To do this, in the tab **Facet Definitions**, click the **New** button to enter a new facet to identify a variable region. Enter the Facets **customLeft** and **customRight** as shown in the following screenshot:

![](img/fccf6c511126cf79e98510edd3b343ac_124_0.png)

6. Press the **OK** button to generate the template, which will be displayed in the main window of the tool.

7. Choose and drag to the template the **Panel Group Layout** component, which is in the **Layout** category of the Component Palette.

![](img/fccf6c511126cf79e98510edd3b343ac_125_0.png)

8. In the Property Inspector, change the Layout property to scroll. With this change, the scroll bars will appear in order to display the full content of the page.

![](img/fccf6c511126cf79e98510edd3b343ac_125_1.png)

## Design and Personalization

9. Go to the Component Palette and drag a **Panel Splitter** component to the center of the page.

![](img/fccf6c511126cf79e98510edd3b343ac_126_0.png)

10. Go to **Property Inspector** and select the **Common** category and change the value of the **Orientation** field to **vertical**.

![](img/fccf6c511126cf79e98510edd3b343ac_126_1.png)

11. Next, choose the **Style** category, press the tab **Box**, and change the **Width** and **Height** properties to the value of 100 percent.

![](img/fccf6c511126cf79e98510edd3b343ac_126_2.png)

12. The template will have generated two regions so far, called first and second. The first will show the data from the header, while the second will have the components associated with the variable regions, used by the pages that will be based on the template.

![](img/fccf6c511126cf79e98510edd3b343ac_127_0.png)

13. Place your cursor between the first and second regions and change their proportions, such as shown in the following screenshot:

![](img/fccf6c511126cf79e98510edd3b343ac_127_1.png)

14. Now, we’re going to divide the second region into two parts. For this, again drag a Panel Splitter component into the second region.

## 15. In the Property Inspector, select the Style category, select the Box tab, and change the Width and Height to 100 percent, shown as follows:

![](img/fccf6c511126cf79e98510edd3b343ac_128_0.png)

## 16. With the last change, we see our template divided into three regions. It is necessary to identify the variable regions, according to facets we defined in Section 5. For this, go to the Common category of the Component Palette and drag the Facet Ref component into the region located on the left side of the template; it will launch a window where we must choose the custom facet we defined previously. Choose customLeft as shown in the following screenshot and click the OK button.

![](img/fccf6c511126cf79e98510edd3b343ac_128_1.png)

17. Repeat the same procedure in the right region using the **customRight** facet. We end up with the following template:

![](img/fccf6c511126cf79e98510edd3b343ac_129_0.png)

18. At the top of our template, we will place the logo of our company. For this, drag an **Image** component from the Component Palette.

![](img/fccf6c511126cf79e98510edd3b343ac_129_1.png)

19. Then a floating window appears; click on the **edit** option of the **Source** field and select the file that has the logo of our company (you can use the file `logo.gif`, which is part of the files for this book). After you select the file, click **Finish**.

![](img/fccf6c511126cf79e98510edd3b343ac_130_0.png)

20. We will finish the development of our template, by setting a background color. In order to do this, select the Structure Palette (located on the lower left side of JDeveloper) and choose the reference **af: panelGroupLayout - Scroll**, as shown in the following screenshot:

![](img/fccf6c511126cf79e98510edd3b343ac_130_1.png)

21. Go to the **Property Inspector**, select the **Style and Theme** category, click the arrow button of the **StyleClass** field, and choose the **Edit** option.

![](img/fccf6c511126cf79e98510edd3b343ac_131_0.png)

22. In the **Edit Property** window, select the **OraBGColorVeryDark** style and click **OK**.

![](img/fccf6c511126cf79e98510edd3b343ac_131_1.png)

23. Finally, our template should have the following appearance:

![](img/fccf6c511126cf79e98510edd3b343ac_131_2.png)

## Building pages with database interaction

At this point, we have a custom template for all our pages. Now, let's use it for building a page with database access. As always, we start with a brief overview of the technologies to use.

## ADF Business Components

ADF Business Components is a framework for implementing a persistence and business layer in our JEE applications. It's easy to use and improves the productivity. Furthermore, we can add business logic over the components.

The architecture of ADF Business Components includes the following components:

- **Entity**: This is a Java Component that represents a specific source of information (that is, a database table).
- **View Object**: This component will be used to implement queries in order to access the data.
- **Application Module**: The principal business rules of our applications will be defined in this component. Generally, we have an application module for each case we use.

> For more detail about ADF Business Components see the _Fusion Developer's Guide, Part II Building Your Business Services_ http://download.oracle.com/docs/cd/E12839_01/ web.1111/b31974.pdf.

## Creating a persistence layer

First, we will create a schema and execute the relevant script. Make sure you have downloaded the script from the book's website into your directory. This script will create data and structures that will be used in our application.

## Creating a Schema for our application

For this exercise, it is necessary to have an Oracle database. The following exercise is made using an Oracle 10g Express Edition database.

Using the command line of your database, run the following instructions:

1. Connect to the database as a dba user, like `sys` or `system`.



## Developing the Web Application

Since we have already defined the name of the Java class that will be used for creating the menu, the next thing we do is implement it. The idea behind the `HorizontalMenu` class is that it is used to read a properties file that holds the menus that are going to be on the web page.

We add properties to a file named `dwrappplication.properties`, and we create it in the same `samples`-package as the `HorizontalMenu`-class. The properties file for the menu items is as follows:

```
menu.1=Tables and lists,TablesAndLists
menu.2=Field completion,FieldCompletion
```

The syntax for the menu property is that it contains two elements separated by a comma. The first element is the name of the menu item. This is visible to user. The second is the name of HTML template file that will hold the page content of the menu item.

The class contains just one method, which is used from JavaScript and via DWR to retrieve the menu items. The full class implementation is shown here:

```
package samples;

import java.io.IOException;
import java.io.InputStream;
import java.util.List;
import java.util.Properties;
import java.util.Vector;

public class HorizontalMenu {
    public HorizontalMenu() {
    }

    public List<String> getMenuItems() throws IOException {
        List<String> menuItems = new Vector<String>();
        InputStream is = this.getClass().getClassLoader().
            getResourceAsStream(
                "samples/dwrappplication.properties");
        Properties appProps = new Properties();
        appProps.load(is);
        is.close();
        for (int menuCount = 1; true; menuCount++) {
            String menuItem = appProps.getProperty("menu." + menuCount);
            if (menuItem == null) {
                break;
            }
            menuItems.add(menuItem);
        }
        return menuItems;
    }
}
```

The implementation is straightforward. The getMenuItems() method loads properties using the ClassLoader.getResourceAsStream() method, which searches the class path for the specified resource. Then, after loading properties, a for loop is used to loop through menu items and then a List of String-objects is returned to the client. The client is the JavaScript callback function that we will see later. DWR automatically converts the List of String objects to JavaScript arrays, so we don’t have to worry about that.

## Testing the Web Application

We haven’t completed any client-side code now, but let’s test the code anyway. Testing uses the Geronimo test environment.

- 1. The Project context menu has the Run As menu that we use to test the application as shown in the following screenshot:

![](img/5b721cfaa97eb697efc92e3914f62fca_69_0.png)

2. **Run on Server** opens a wizard to define a new server runtime. The following screenshot shows that the Geronimo test environment has already been set up, and we just click **Finish** to run the application. If the test environment is not set up, we can manually define a new one in this dialog:

![](img/5b721cfaa97eb697efc92e3914f62fca_70_0.png)

3. After we click **Finish**, Eclipse starts the Geronimo test environment and our application with it. When the server starts, the **Console** tab in Eclipse informs us that it's been started.

![](img/5b721cfaa97eb697efc92e3914f62fca_70_1.png)

## User Interface: Basic Elements

The Servers tab shows that the server is started and all the code has been synchronized, that is, the code is the most recent (Synchronization happens whenever we save changes on some deployed file.) The Servers tab also has a list of deployed applications under the server. Just the one application that we are testing here is visible in the Servers tab.

![](img/5b721cfaa97eb697efc92e3914f62fca_71_0.png)

Now comes the interesting part – what are we going to test if we haven't really implemented anything? If we take a look at the web.xml file, we will find that we have defined one initialization parameter. The Debug parameter is true, which means that DWR generates test pages for our remoted Java classes. We just point the browser (Firefox in our case) to the URL http://127.0.0.1:8080/DWREasyAjax/dwr and the following page opens up:

![](img/5b721cfaa97eb697efc92e3914f62fca_71_1.png)

This page will show a list of all the classes that we allow to be remoted. When we click the class name, a test page opens as in the following screenshot:

Then there is the possibility of testing remoted methods. When we test our own method, getMenuItems(), we see a response in an alert box:

![](img/5b721cfaa97eb697efc92e3914f62fca_73_0.png)

The array in the alert box in the screenshot is the JavaScript array that DWR returns from our method.

## Developing Web Pages

The next step is to add the web pages. Note that we can leave the test environment running. Whenever we change the application code, it is automatically published to test the environment, so we don't need to stop and start the server each time we make some changes and want to test the application.

The CSS style sheet is from the Dynamic Drive CSS Library. The file is named styles.css, and it is in the WebContent directory in Eclipse IDE. The CSS code is as shown:

```
/*URL: http://www.dynamicdrive.com/style/ */
.basictab{
padding: 3px 0;
margin-left: 0;
font: bold 12px Verdana;
border-bottom: 1px solid gray;
list-style-type: none;
text-align: left; /*set to left, center, or right to align the menu as desired*/
}
.basictab li{
display: inline;
margin: 0;
}
```

```
.basictab li a{
text-decoration: none;
padding: 3px 7px;
margin-right: 3px;
border: 1px solid gray;
border-bottom: none;
background-color: #f6ffd5;
color: #2d2b2b;
}

.basictab li a:visited{
color: #2d2b2b;
}

.basictab li a:hover{
background-color: #DBFF6C;
color: black;
}

.basictab li a:active{
color: black;
}

.basictab li.selected a{ /*selected tab effect*/
position: relative;
top: 1px;
padding-top: 4px;
background-color: #DBFF6C;
color: black;
}
```

This CSS is shown for the sake of completion, and we will not go into details of CSS style sheets. It is sufficient to say that CSS provides an excellent method to create websites with good presentation.

The next step is the actual web page. We create an index.jsp page, in the WebContent directory, which will have the menu and also the JavaScript functions for our samples. It should be noted that although all JavaScript code is added to a single JSP page here in this sample, in "real" projects it would probably be more useful to create a separate file for JavaScript functions and include the JavaScript file in the HTML/JSP page using a code snippet such as this:
<script type="text/javascript" src="myjavascriptcode/HorizontalMenu.js"/>.

We will add JavaScript functions later for each sample. The following is the JSP code that shows the menu using the remoted `HorizontalMenu` class.

```
```jsp
<%@ page language="java" contentType="text/html; charset=ISO-8859-1"
         pageEncoding="ISO-8859-1"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
"http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
<link href="styles.css" rel="stylesheet" type="text/css"/>
<script type='text/javascript' src='/DWREasyAjax/dwr/engine.js'></script>
<script type='text/javascript' src='/DWREasyAjax/dwr/util.js'></script>
<script type='text/javascript' src='/DWREasyAjax/dwr/interface/HorizontalMenu.js'></script>
<title>DWR samples</title>
<script type="text/javascript">

function loadMenuItems()
{
    HorizontalMenu.getMenuItems(setMenuItems);
}

function getContent(contentId)
{
    AppContent.getContent(contentId,setContent);
}

function menuItemFormatter(item)
{
    elements=item.split(',');
    return '<li><a href="#" onclick="getContent(\''+elements[1]+'\');return false;">'+elements[0]+'</a></li>';
}

function setMenuItems(menuItems)
{
    menu=dwr.util.byId("dwrMenu");
    menuItemsHtml='';
    for(var i=0;i<menuItems.length;i++)
    {
        menuItemsHtml=menuItemsHtml+menuItemFormatter(menuItems[i]);
    }
    menu.innerHTML=menuItemsHtml;
}

function setContent(htmlArray)
{
{
```
```

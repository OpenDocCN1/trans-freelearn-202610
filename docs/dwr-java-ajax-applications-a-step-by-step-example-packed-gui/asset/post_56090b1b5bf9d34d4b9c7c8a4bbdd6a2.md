
```javascript
var contentFunctions='';
var scriptToBeEvaled='';
var contentHtml='';
for(var i=0;i<htmlArray.length;i++)
{
    var html=htmlArray[i];
    if(html.toLowerCase().indexOf('<script>')>-1)
    {
        if(html.indexOf('TO BE EVALED')>-1)
        {
            scriptToBeEvaled=html.substring(html.indexOf('>')+1,
                                            html.indexOf('</'));
        }
        else
        {
            eval(html.substring(html.indexOf('>')+1,html.indexOf('</')));
            contentFunctions+=html;
        }
    }
    else
    {
        contentHtml+=html;
    }
}

contentScriptArea=dwr.util.byId("contentAreaFunctions");
contentScriptArea.innerHTML=contentFunctions;
contentArea=dwr.util.byId("contentArea");
contentArea.innerHTML=contentHtml;
if(scriptToBeEvaled!='')
{
    eval(scriptToBeEvaled);
}
}
</script>
</head>
<body onload="loadMenuItems()">
<h1>DWR Easy Java Ajax Applications</h1>
<ul class="basictab" id="dwrMenu">
</ul>
<div id="contentAreaFunctions">
</div>
<div id="contentArea">
</div>
</body>
</html>
```

This JSP is our user interface. The HTML is just normal HTML with a head element and a body element. The head includes reference to a style sheet and to DWR JavaScript files, engine.js, util.js, and our own HorizontalMenu.js. The util.js file is optional, but as it contains very useful functions, it could be included in all the web pages where we use the functions in util.js.

The body element has a contentArea place holder for the content pages just below the menu. It also contains the content area for JavaScript functions for a particular content. The body element onload-event executes the loadMenuItems() function when the page is loaded. The loadMenuItems() function calls the remoted method of the HorizontalMenu Java class. The parameter of the HorizontalMenu.getMenuItems() JavaScript function is the callback function that is called by DWR when the Java method has been executed and it returns menu items.

The setMenuItems() function is a callback function for the loadMenuItems() function mentioned in the previous paragraph. While loading menu items, the HorizontalMenu.getMenuItems() remoted method returns menu items as a List of Strings as a parameter to the setMenuItems() function. The menu items are formatted using the menuItemFormatter() helper function.

The menuItemFormatter() function creates li elements of menu texts. Menus are formatted as links, (a href) and they have an onclick event that has a function call to the getContent-function, which in turn calls the AppContent.getContent() function.

The AppContent is a remoted Java class, which we haven't implemented yet, and its purpose is to read the HTML from a file based on the menu item that the user clicked. Implementation of AppContent and the content pages are described in the next section.

Our dynamic user interface looks like this:

![](img/5b721cfaa97eb697efc92e3914f62fca_78_0.png)

Note the Firebug window at the bottom of the browser screen. The Firebug console in the screenshot shows one POST request to our HorizontalMenu.getMenuItems() method. Other Firebug features are extremely useful during development work, and we find it useful that Firebug has been enabled throughout the development work.

## Callback Functions

We saw our first callback function as a parameter in the HorizontalMenu.getMenuItems(setMenuItems) function, and since callbacks are an important concept in DWR, it would be good to discuss a little more about them now that we have seen their first usage.

Callbacks are used to operate on the data that was returned from a remoted method. As DWR and AJAX are asynchronous, typical return values in RPCs (Remote Procedure Calls), as in Java calls, do not work. DWR hides the details of calling the callback functions and handles everything internally from the moment we return a value from the remoted Java method to receiving the returned value to the callback function.

Two methods are recommended while using callback functions.

We have already seen the first method in the `HorizontalMenu.getMenuItems(setMenuItems)` function call. Remember that there are no parameters in the `getMenuItems()` Java method, but in the JavaScript call, we added the callback function name at the end of the parameter list. If the Java method has parameters, then the JavaScript call is similar to `CountryDB.getCountries(selectedLetters,setCountryRows)`, where selectedLetters is the input parameter for the Java method and setCountryRows is the name of the callback function (we see the implementation later on).

The second method to use callbacks is a meta-data object in the remote JavaScript call. An example (a full implementation is shown later in this chapter) is shown here:

```
CountryDB.saveCountryNotes(ccode,newNotes, {
    callback:function(newNotes)
    {
        //function body here
    }
});
```

Here, the function is anonymous and its implementation is included in the JavaScript call to the remoted Java method. One advantage here is that it is easy to read the code, and the code is executed immediately after we get the return value from the Java method. The other advantage is that we can add extra options to the call.

Extra options include timeout and error handler as shown in the following example:

```
CountryDB.saveCountryNotes(ccode,newNotes, {
    callback:function(newNotes)
    {
        //function body here
    },
    timeout:10000,
    errorHandler:function(errorMsg) { alert(errorMsg); }
});
```

It is also possible to add a callback function to those Java methods that do not return a value. Adding a callback to methods with no return values would be useful in getting a notification when a remote call has been completed.

## Afterword

Our first sample is ready, and it is also the basis for the following samples. We also looked at how applications are tested in the Eclipse environment.

Using DWR, we can look at JavaScript code on the browser and Java code on the server as one. It may take a while to get used to it, but it will change the way we develop web applications. Logically, there is no longer a client and a server but just a single run time platform that happens to be physically separate. But in practice, of course, applications using DWR, JavaScript on the client and Java in the server, are using the typical client-server interaction. This should be remembered when writing applications in the logically single run-time platform.

## Implementing Tables and Lists

The first actual sample is very common in applications: tables and lists. In this sample, the table is populated using the DWR utility functions, and a remoted Java class. The sample code also shows how DWR is used to do inline table editing. When a table cell is double-clicked, an edit box opens, and it is used to save new cell data.

The sample will have country data in a CSV file: country Name, Long Name, two-letter Code, Capital, and user-defined Notes. The user interface for the table sample appears as shown in the following screenshot:

![](img/5b721cfaa97eb697efc92e3914f62fca_80_0.png)

### Server Code for Tables and Lists

The first thing to do is to get the country data. Country data is in a CSV file (named `countries.csv` and located in the samples Java package). The following is an excerpt of the content of the CSV file (data is from `http://www.state.gov`).

```
Short-form name,Long-form name,FIPS Code,Capital

Afghanistan,Islamic Republic of Afghanistan,AF,Kabul

Albania,Republic of Albania,AL,Tirana

Algeria,People's Democratic Republic of Algeria,AG,Algiers

Andorra,Principality of Andorra,AN,Andorra la Vella

Angola,Republic of Angola,AO,Luanda

Antigua and Barbuda,(no long-form name),AC,Saint John's

Argentina,Argentine Republic,AR,Buenos Aires

Armenia,Republic of Armenia,AM,Yerevan

...
```

The CSV file is read each time a client requests country data. Although this is not very efficient, it is good enough here. Other alternatives include an in-memory cache or a real database such as Apache Derby or IBM DB2. As an example, we have created a CountryDB class that is used to read and write the country CSV. We also have another class, DBUtils, which has some helper methods. The DBUtils code is as follows:

```
package samples;

import java.io.BufferedReader;
import java.io.File;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;
import java.io.InputStream;
```

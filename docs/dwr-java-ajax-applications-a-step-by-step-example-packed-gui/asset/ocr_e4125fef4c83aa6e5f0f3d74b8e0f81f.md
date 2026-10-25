Then there is the possibility of testing remoted methods. When we test our own method, getMenuItems(), we see a response in an alert box:

![](bbox=[0.26, 0.15, 0.74, 0.31])

The array in the alert box in the screenshot is the JavaScript array that DWR returns from our method.

# Developing Web Pages

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
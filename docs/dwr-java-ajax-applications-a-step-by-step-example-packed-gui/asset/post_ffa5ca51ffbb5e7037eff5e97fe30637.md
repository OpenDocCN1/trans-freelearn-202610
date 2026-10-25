
## User Interface: Advanced Elements

```java
String[] expDate = expiryDate.split("/");
try {
    int month = Integer.parseInt(expDate[0]);
    int year = Integer.parseInt(expDate[1]);
    if ((month >= 1 && month <= 12) && (year >= 8 && year
        <= 99)) {
        return true;
    }
    return false;
} catch (Exception e) {
    e.printStackTrace();
    return false;
}
}
/*
 * The next two methods perform credit card validation.
 * Only the format is checked, it is not checked
 * against a card holder.
 *
 * Methods provided by Michael Gilleland in his essay
 * "Anatomy of Credit Card Numbers"
 * http://www.merriampark.com/anatomycc.htm
 */
private String getDigitsOnly(String s) {
    StringBuffer digitsOnly = new StringBuffer();
    char c;
    for (int i = 0; i < s.length(); i++) {
        c = s.charAt(i);
        if (Character.isDigit(c)) {
            digitsOnly.append(c);
        }
    }
    return digitsOnly.toString();
}

public boolean isValidCreditCard(String cardNumber) {
    String digitsOnly = getDigitsOnly(cardNumber);
    int sum = 0;
    int digit = 0;
    int addend = 0;
    boolean timesTwo = false;
```

```
for (int i = digitsOnly.length() - 1; i >= 0; i--) {
    digit = Integer.parseInt(digitsOnly.substring(i, i + 1));
    if (timesTwo) {
        addend = digit * 2;
        if (addend > 9) {
            addend -= 9;
        }
    } else {
        addend = digit;
    }
    sum += addend;
    timesTwo = !timesTwo;
}
int modulus = sum % 10;
return modulus == 0;
```

The first method, submitOrder(), takes the name, address, and credit card info as parameters and validates the input before sending the order to the order-processing system (not implemented in this sample) and returns either true or false value based on whether the order was successful or not.

The second method, checkExpiryFormat(), takes the credit card expiry date as the parameter and validates that it is in the correct format, MM/YY where MM and YY are integers in the range of 1-12 and 8-99 (years 2008-2099) respectively.

The last two methods validate that the credit card number is in the correct format. The code is written by Michael Gilleland, and it was published in his essay Anatomy of Credit Card Numbers (http://www.merriampark.com/anatomycc.htm). The essay explains what all those numbers in a credit card are, and how to verify that a given number is really a credit card number using the Luhn algorithm (refer to the essay for more information).

## Testing the Form

The next thing to do is to add FormHandler to the HTML page and the remoted class to dwr.xml. We add the following to the index.jsp file:

```
<script type='text/javascript' src='/DWREasyAjax/dwr/interface/FormHandler.js'></script>
```

## User Interface: Advanced Elements

And we add the following to the dwr.xml file:

```
```xml
<create creator="new" javascript="FormHandler">
    <param name="class" value="samples.FormHandler"/>
    <include method="checkExpiryFormat"/>
    <include method="isValidCreditCard"/>
    <include method="submitOrder"/>
</create>
```
```

Now the order form is ready. It is a very simple order form, but the idea is clear. Form fields can be validated while the user is inputting the text. So when the user submits the form, it is most likely that all the fields are correct. The following screenshot shows the form filling procedure in progress:

![](img/5b721cfaa97eb697efc92e3914f62fca_113_0.png)

Note how the credit card expiry field is left incomplete. The Firebug window shows the DWR requests, and each time a character is entered in the field, the `checkExpiryFormat()` method is called. Note that, in this case, calling the server each time a character is entered introduces a lot of network traffic and overhead to the server code. However, in this sample, an illustrative example is worth the extra overhead on the server. Perhaps also in other cases, overhead may be considered secondary to good user experience and immediate feedback to the user.

When the fields are correct and the order is submitted, order is sent to the order processing system. After the order is completed, the user gets the confirmation for a successful order as shown in the following screenshot:

![](img/5b721cfaa97eb697efc92e3914f62fca_114_0.png)

Of course, in real life, the user would get a receipt for the order instead of the green **Order submitted** text.

## Afterword

The sample form in this section shows how DWR is used in form handling. DWR uses on forms include real-time validation of form fields. We also saw the algorithm to check the validity of the credit card number.

## Building a Navigation Tree

This is a sample for building a navigation menu, where the menu contents are fetched from the server using DWR. The following is a screenshot of the menu we are developing in this section:

![](img/5b721cfaa97eb697efc92e3914f62fca_115_0.png)

## Developing the User Interface

We have already added the navigation tree menu item to dwrappplication.properties in the previous section, so we can start the development with the HTML file. The following HTML file is in the samples directory, and its name is NavigationTree.html.

```
<h3>Navigation Tree</h3>
<p>Simple navigation tree example.</p>
<table border="1" cellspacing="5" cellpadding="5">
```

```
<tr>
    <td><div class="wireframemenu" id="treeMenu"></div></td>
    <td><div id="treeMenuContent"></div></td>
</tr>
</table>

<script type='text/javascript'>
//TO BE EVALED
NavigationTree.getRootElements(createTreeMenu);
</script>
```

Note that the tree menu is in the div element with the treeMenu ID. It also has a style class, wireframemenu. This style is from the Dynamic Drive CSS library. The following styles are added to the styles.css file.

```
/*Credits: Dynamic Drive CSS Library */
/*URL: http://www.dynamicdrive.com/style/ */

.wireframemenu{
border: 1px solid #C0C0C0;
background-color: white;
border-bottom-width: 0;
width: 170px;
}

* html .wireframemenu{ /*IE only rule. Original menu width minus all left/right paddings */
width: 164px;
}

.wireframemenu ul{
padding: 0;
margin: 0;
list-style-type: none;
}

.wireframemenu a{
font: bold 13px Verdana;
padding: 4px 3px;
display: block;
width: 100%; /*Define width for IE6's sake*/
color: #595959;
text-decoration: none;
border-bottom: 1px solid #C0C0C0;
}
```

```
.wireframemenu a:visited{
color: #595959;
}

html>body .wireframemenu a { /*Non IE rule*/
width: auto;
}

.wireframemenu a:hover{
background-color: #F8FBBD;
color: black;
}
```

## Creating the NavigationTree Class

In the NavigationTree.html file, there was a JavaScript call to NavigationTree.getRootElements(). This is a remot ed Java class, so let's implement the Java class. The source code is as shown:

```
package samples;

import java.util.Hashtable;
import java.util.List;
import java.util.Map;
import java.util.Random;
import java.util.Vector;

public class NavigationTree {
    private String[] rootLevel={"Northern Hemisphere","Southern Hemisphere"};

    private Map<String,List<String>> level_1=new Hashtable<String,List<String>>();
    private Map<String,List<String>> level_2=new Hashtable<String,List<String>>();

    private String[] ncap={"Helsinki","Stockholm","Oslo"};
    private String[] scap={"Wellington","Canberra","Port Louis"};

    public NavigationTree()
    {
        List<String> countries=new Vector<String>();
        //set northern hemisphere
        countries.add("Finland");
        countries.add("Sweden");
        countries.add("Norway");
```

```
```javascript
var html="<ul>";

for (index in rootElements)
{
    var element=rootElements[index];
    NavigationTree.hasChildren(element,{
        async:false,
        callback:function(hasChildren)
        {
            if(hasChildren)
            {
                html+='<li><a href="#" onclick="expandElement(this);\n                              return false;"> + '+element+'</a></li>';
            }
            else
            {
                html+='<li><a href="#" onclick="expandElement(this);\n                              return false;">'+element+'</a></li>';
            }
        }
    });
}

html+="</ul>";
treeMenu.innerHTML=html;

}

function expandElement(element)
{
    var value=element.text.replace(' + ','');
    var html="";
    NavigationTree.getChildren(value,{
        async:false,
        callback:function(childElements)
        {
            for (index in childElements)
            {
                var element=childElements[index];
                NavigationTree.hasChildren(element,{
                    async:false,
                    callback:function(hasChildren)
                    {
                        if(hasChildren)
```
```


The showPopupMenu() function is called each time a user enters a letter in the input box. The function gets the value of the input field and calls the LicenseDB.getLicensesStartingWith() method. The callback function is specified in the function parameters. The callback function gets all the licenses that match the parameter, and based on the length of the parameter (which is an array), it either shows a pop-up box with all the matching license names, or, if the array length is one, hides the pop-up box and inserts the full license name in the text field. In the pop-up box, the license names are wrapped within the div element that has an onclick event listener that calls the completeEditBox() function.

The hidePopupMenu() function just closes the pop-up menu and the completeEditBox() function inserts the clicked license text in the input box and moves the focus to the button. The showLicenseText() function is called when we click the Show license text button. The function calls the LicenseDB.getLicenseContentUrl() method and the callback function creates an iframe element to show the license content directly from http://www.opensource.org, as shown in the following screenshot:

![](img/5b721cfaa97eb697efc92e3914f62fca_103_0.png)

### Afterword

Field completion improves user experience in web pages and the sample code in this section showed one way of doing it using DWR.

It should be noted that the sample for field completion presented here is only for demonstration purposes.

### Summary

This chapter provided samples for a couple of common tasks that are used in web development: tables and lists, field completion, and even a generic frame, called a dynamic user interface, for our sample code. Both the tables and lists sample and the field completion sample had a very simple CSV-based "database" that holds the data for our purposes, and both had a remoted method that DWR uses to get the data from the server and show it in the client.

We also saw some good examples of HTML, CSS, and JavaScript. In fact, without knowledge of JavaScript it would be difficult to write web applications.

Many years ago, as some of you may remember, JavaScript was a dirty word in web development and no self-respecting developer would touch JavaScript. But change is a part of life, and in this case, change has been for the better. JavaScript and Java work very well together with the DWR in between.

The next chapter continues with the user interface part, and shows a couple more samples, including a map scrolling functionality, similar to what is found in the popular Google Maps website.

## 5 User Interface: Advanced Elements

This chapter has three samples: forms, navigation tree, and map scrolling. Forms are found in almost all the web applications, and the sample shows how to do form validation and processing using DWR. The navigation tree includes a simple navigation tree that has a Java bean for holding menu elements that are accessed via DWR. The last sample in this chapter is a map scrolling sample that shows how to achieve zooming and scrolling of a map using Java, JavaScript, and DWR.

The following are the sections discussed in this chapter:

- Creating Forms — includes examples of form processing
- Building a Navigation Tree — shows one way of using DWR in the navigation tree implementation
- Map Scrolling Using DWR — shows the DWR functionality in a web page map (from the planet Mars in this sample)

### Creating Forms

This sample is about forms, form validation, and submission of a form. For this sample, the form we will handle is very simple, just a few fields, and the point of this sample is to show form validation using DWR and form submission using DWR. The form will appear as shown in the following screenshot:

![](img/5b721cfaa97eb697efc92e3914f62fca_107_0.png)

#### Developing the User Interface

The **Name** and **Address** fields are nothing special, except maybe that the address field in real-life is virtually never a simple text field. The **Credit card** number is validated when it is entered in the input field. The **Credit card expiry** field is also validated when it is entered.

The first thing to do is to add new menu items to the Dynamic User Interface that we had designed in the previous chapter. This is done by adding properties for menu items to the `dwapplication.properties` file. We have already added menu items for the navigation tree and the map scrolling samples that we will develop later in this chapter:

```
menu.3=Form handling,FormHandling
menu.4=Navigation tree,NavigationTree
menu.5=Map scrolling,MapScrolling
```

The HTML code for our FormHandling.html file looks like this:

```
<h3>Form handling</h3>
<p>Simple order form.</p>
<div id="formFeedback"></div>
<table border="1" cellspacing="5" cellpadding="5">
  <tr>
    <td>Name</td>
    <td><input type="text" id="nameEditBox" value=""
        size="40" /></td>
  </tr>
  <tr>
    <td>Address</td>
    <td><input type="text" id="addressEditBox" value="" size="40" /></td>
  </tr>
  <tr>
    <td>Credit card #<br><a href="#" onclick="useTestCreditCardNumber();return false;">use test number</a></td>
    <td><input type="text" id="creditCardEditBox" value="" onkeyup="isValidCreditCard()" size="40" /></td>
  </tr>
  <tr>
    <td>Credit card expiry (mm/yy)</td>
    <td><input type="text" id="expiryEditBox" value="" onkeyup="checkExpiryDate()" size="40" /></td>
  </tr>
  <tr>
    <td>&nbsp;</td>
    <td><input type="button" id="submitOrderButton" value="Submit order" onclick="submitOrder()" /></td>
  </tr>
</table>
```

JavaScript functions are specified in the credit card number and expiry date input field onkeyup events and the onclick events of the use test number link and the submit button. The following JavaScript functions are in the index.jsp file:

```
function useTestCreditCardNumber()
{
    var creditCardEditBox=dwr.util.byId('creditCardEditBox');
    creditCardEditBox.value='555555555554444';
}

function isValidCreditCard()
{
    var creditCardEditBox=dwr.util.byId('creditCardEditBox');
    creditCardNumber=creditCardEditBox.value;
    FormHandler.isValidCreditCard(creditCardNumber,{
        callback:function(valid)
        {
            var feedback=dwr.util.byId('formFeedback');
            var html="";
            if(!valid)
            {
                html='<p><font color="red">Credit card number is not valid.</font></p>';
            }
            feedback.innerHTML=html;
        }
    });
}
```

The first function, useTestCreditCardNumber, inserts a test number into the credit card field, so we don't have to use our own card number.

The isValidCreditCard() function is called when something is entered in the credit card input field. The function calls a server-side Java method (to be implemented shortly) and receives either true or false. If the credit card number fails the validation, the user is informed by changing the content of the div element with the formFeedback ID.

The checkExpiryDate() function is similar to the previous function except that it checks the expiry date syntax.

The last function is the submitOrder() function that reads form field values and calls the submitOrder() method in a remoted Java class. The Java method returns true or false, and the status of order submission is shown to the user.

#### Creating the FormHandler Class

The next thing to do is the FormHandler Java class. The implementation of the class is as follows:

```
package samples;

public class FormHandler {
    public boolean submitOrder(String name,String address,String creditCardNumber,String expiryDate)
    {
        boolean validExpiryDate=checkExpiryFormat(expiryDate);
        boolean validCreditCardNumber=
            isValidCreditCard(creditCardNumber);
        //submit order to orderprocessing system
        boolean orderSubmitted=true;

        return validCreditCardNumber && validExpiryDate &&
            orderSubmitted;
    }

    public boolean checkExpiryFormat(String expiryDate) {
        if (expiryDate.length() != 5) {
            return false;
        }
        if (expiryDate.indexOf("/") == -1) {
            return false;
        }
    }
}
```

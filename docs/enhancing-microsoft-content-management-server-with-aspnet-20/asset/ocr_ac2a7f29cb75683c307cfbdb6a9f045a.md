# The Checkout Screen

The checkout page will receive the values entered in the ordering screen and display them on the screen. When a user is satisfied that the information is correct, he or she clicks on the **Checkout** button, and a message appears informing her or him that the order has been successfully placed.

+   1. Add a web form to the `Forms` folder. Name the form `checkout.aspx` and set `tropicalgreen.master` to be the master page.
+   2. In **Source** view, add the following code between the `<asp:Content>` tags:

```
<h1>Checkout</h1>
<table>
    <tr>
        <td>First Name:</td>
        <td>(Space for First Name Label)</td>
    </tr>
    <tr>
        <td>Last Name:</td>
        <td>(Space for Last Name Label)</td>
    </tr>
    <tr>
        <td>Address:</td>
        <td>(Space for Address Label)</td>
    </tr>
    <tr>
        <td>Plant:</td>
        <td>(Space for Plant Label)</td>
    </tr>
    <tr>
        <td colspan="2" align="right">
            (Space for Confirm Button)
        </td>
    </tr>
</table>
(Space for Message Label)
```
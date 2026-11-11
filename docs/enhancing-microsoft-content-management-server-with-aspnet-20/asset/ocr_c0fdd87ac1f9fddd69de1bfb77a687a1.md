2. Create a channel directly under the **tropicalgreen** channel. Give the new channel the following property values:

| Property | Property Value |
|---|---|
| Name | OrderPlant |
| DisplayName | Order Plants |
| Script URL | /tropicalgreen/forms/order.aspx |

3. Once the channel has been created, create a selection type custom property named **MenuLocation** and assign it the following options:

+   - Top
+   - Right (make it the default value)

The "Order a Plant" menu item should now appear on the left menu.

![](bbox=[0.425, 0.4, 0.575, 0.49])

# Why Cross-Page Postbacks Do Not Work on MCMS Sites

Navigate to any page within the website and click **Order a Plant**. In the ordering page, enter your first name, last name, and address, select a plant, and click on the **Order** button.

The page will refresh. The page seems to be posting back to itself even though we have set the `PostBackUrl` property of the button to a different page!

To understand why cross-page postbacks do not work on MCMS pages, let’s take a look at the client script of the generated page. Observe the code generated for the `btnOrder` button (right-click anywhere on the page and select **View Source**), which resembles the following code:

```
<input type="submit"
name="ctl00$ContentPlaceHolder1$btnOrder"
value="Order"
onclick="javascript:
```
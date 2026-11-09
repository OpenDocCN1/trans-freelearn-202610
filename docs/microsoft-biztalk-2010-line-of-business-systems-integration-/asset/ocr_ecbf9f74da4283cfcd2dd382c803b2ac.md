2. We will populate each of these message parts from within the **SQLConfiguration_to_GetListItemsRequest.btm** map. In the **listName** node, we want to populate the GUID for our Custom List. In this case, we are going to leverage the **ListGUID** element in the **msgSQLConfiguration** message by mapping this node to the **listName** node.

![](bbox=[0.225, 0.235, 0.775, 0.325])

3. For the **viewName** property, we are simply going to assign an empty string value via a **String Concatenation** Functoid. In our scenario, we are not leveraging SharePoint views.

![](bbox=[0.36, 0.395, 0.64, 0.445])

4. In the **query** node of the SharePoint Request message, notice the type of node that it is expecting: **<Any>**. Within the context of this schema, this means it is a loosely typed node and is expecting us, the developer, to provide valid CAML mark up. This CAML mark up will then get executed against our Custom List. If we wanted to filter the SharePoint List based upon a set of criteria, we would do so here. Since we are interested in all data stored within the list, we will look for all records where the **ID** field is greater than 0. Since ID is a system column, it is guaranteed to be there and IDs always start at number 1 so we will always have records returned if they exist in the list.

![](bbox=[0.355, 0.625, 0.645, 0.675])

5. In order to populate this CAML query, we can leverage a **Scripting Functoid** and have it return XML from an XSLT Call Template. We can configure this **Scripting Functoid** by right mouse clicking on it and selecting **Configure Functoid Script**.
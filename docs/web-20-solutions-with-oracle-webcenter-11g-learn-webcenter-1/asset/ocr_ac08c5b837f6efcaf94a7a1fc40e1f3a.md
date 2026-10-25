# # Embedding a Discussions task flow in the JSF page

From the Resource Palette, drag the Discussions Forums task flow and drop it on to
the JSF page. When prompted, select Region as the way to create the task flow.

![](bbox=[0.1855, 0.2125, 0.8174, 0.3172])

In the Task Flow Binding dialog, enter the values for the categoryId and forumId.
You can find the IDs using the Jive Forums Admin Console.

![](bbox=[0.2666, 0.3828, 0.7334, 0.5918])

| Parameters   | Description    |
|--------------|----------------|
| categoryId   | The category ID of an existing category in your WebCenter
Discussions instance to which the view should be scoped.    |
| forumId      | The identifier for an existing forum within the specified
category. You should make sure that the specific forum exists. |
| showRecursiveForums | Determines if only the forums in the specified category
will be shown or all the subcategories as well. If set to false
(the default value) then only the forums under the given
category will be shown, otherwise all the forums under the
category and the subcategories will be shown.    |
| isCategoryView | If this is set to true then the task flow will list the forums
grouped under the category. When it is set to false (the
default), the topics associated with the forum will be shown. |
|              |    |
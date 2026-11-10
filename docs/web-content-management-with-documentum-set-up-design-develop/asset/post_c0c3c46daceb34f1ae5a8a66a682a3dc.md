
![](img/b18244e6621ecc39cef03ff6c0e55e7c_148_0.png)

Figure 11.10: Object Type Functionality tab

Figure 11.10 shows the Functionality tab for Object Type in Documentum Application Builder.

5. Under the Events tab (refer to figure 11.11), you are shown the system events (read-only) for the object type.

You can add, modify, and remove application-specific events by clicking on the Add, Remove, and Edit... buttons respectively. These events are used for auditing purposes in the Docbase.

![](img/b18244e6621ecc39cef03ff6c0e55e7c_149_0.png)

Figure 11.11: Object Type Events tab

6. After you have filled in the relevant data in all the necessary tabs, right-click on the object type cust_newsarticle (in the left tree pane) and choose the option Check in selected object(s) to check-in the object type in Docbase. Doing so makes the newly created object type available in the Docbase for applications.

7. Till now the object type cust_newsarticle was not associated with any lifecycle. Let us now associate it with Documentum's Default Lifecycle, so that whenever content is created from this object type, it is automatically attached to the Default Lifecycle.

Insert Default Lifecycle into the TestDocApp by choosing Insert | Object from the Docbase | Document Lifecycle menu option.

Once the Default Lifecycle is included in our DocApp, open it in Edit mode as shown in figure 11.12.

![](img/b18244e6621ecc39cef03ff6c0e55e7c_150_0.png)

Figure 11.12: Documentum Default Lifecycle

8. Please click on the small box (...) shown against the Primary Type field and in the opened popup (refer to figure 11.13), move the cust_newsarticle object type from Available Subtypes to Acceptable Subtypes section by clicking on the → button. Finally, click on OK.

![](img/b18244e6621ecc39cef03ff6c0e55e7c_150_1.png)

Figure 11.13: Select Types popup

Now check-in the modified lifecycle in the Docbase by right-clicking on the lifecycle object (in the left tree pane) and choose the option Check in selected object(s).

Remember to check-in the lifecycle (refer to figure 11.14) in the Docbase as Same version and not as a Major/Minor version. This will prevent creation of multiple Lifecycle versions in the Docbase and hence avoid possible confusions arising out of numerous versions.

![](img/b18244e6621ecc39cef03ff6c0e55e7c_151_0.png)

Figure 11.14: Checking in lifecycle object in the Docbase

9. After the Default Lifecycle has been checked in, the system brings it back to Draft state, thus making it unavailable for use by applications. Open the lifecycle in View mode and click on Validate and then Install to install the lifecycle back in the Docbase.

10. Now we need to specify a default lifecycle for the newly created object type. Open the object type cust_newsarticle in Edit mode and choose Default Lifecycle from the Default Document Lifecycle dropdown as shown in figure 11.15. Check in the modified object type back into the Docbase.

![](img/b18244e6621ecc39cef03ff6c0e55e7c_152_0.png)

Figure 11.15: Setting a lifecycle for the object type

We will be covering lifecycles in detail in the next chapter; so don't worry if you don't understand the relation between object type and lifecycle well.

## 11.2 Designing and Creating Custom Attributes of Object Type(s)

Once you have designed your custom object type, it inherits all the attributes (properties) of its supertype object type. You might want to additionally create some custom attributes that are unavailable in its supertype and that are specific only to this object type.

For our cust_newsarticle object type, we might need the following custom attributes (say):

- News article one line summary
- A flag to identify if this news article is a press release or not
- News article priority: Out of all the published news articles, where in the order this article needs to be displayed on the website.
- News article editor(s) and approver(s) names

It is best to prepare a small table to design the attributes for the custom object type and fill in the respective columns of the table as per your business-specific requirements. Let us discuss this in the context of our dummy website (please refer to figure 11.16):

| Attribute Name | Attribute Label | Data Type | Length | Single/Repeating | Mandatory | Values |
|---|---|---|---|---|---|---|
| cust_news_summary | News Article Summary | String | 200 | Single | Yes | None |
| cust_news_is_press_rel | Is this for press release? | Boolean | 0 | Single | No | None |
| cust_news_display_order | Display Order | Integer | 0 | Single | No | Conditional:
- 1,3,5,7 if News article is for press release
- 0,2,4,6 if News article is not for press release |
| cust_news_edit_approvers | Editors/Approvers | String | 100 | Repeating | No | None |

Figure 11.16: Attributes Design table

One should design the attributes very carefully, since after an attribute has been created and checked in to the Docbase, the following cannot be modified:

- Name of the attribute
- Data type of the attribute
- Whether the attribute is single or repeating
- Length of the attribute (can only be increased)

## 11.3 Adding Attributes to an Object Type

Choose the object type in question within the DocApp and select Insert | Attribute. We will first take an example by creating the attribute cust_news_summary for the object type cust_newsarticle.

Fill in the relevant fields in the new attribute creation screen as shown in the figure 11.17.

![](img/b18244e6621ecc39cef03ff6c0e55e7c_154_0.png)

Figure 11.17: Attribute General tab

### 11.3.1 Attribute Names

Similar to object type names, attribute names have certain restrictions that need to be carefully adhered to. Figure 11.18 lists down some limitations for attribute names.

### 11.3.2 Limitations of Object Type Attribute Names

| Limitations |
|---|
| Should be in lowercase |
| Should not begin with: white space, single quote (') or a numeral |
| Should not begin with 'dm_', 'i_', 'r_', and 'a_' since Documentum internally uses these for its object attribute naming scheme |
| Should not have a name that matches with the following reserved words in Documentum: 'from', 'where' and 'select' |

Figure 11.18: Attribute naming convention limitations

### 11.3.2.1 Label

The Label field will be the actual label shown against this attribute field in the properties page for the object. You can surround the text in HTML markup as well for a visual appeal when displaying the attribute in properties page. We will look more into this in Chapter 24.

### 11.3.2.2 Data Type

Choose the data type for the attribute from the Data type dropdown. The available data types shown in the dropdown are:

- Boolean
- Integer
- String
- ID
- Time
- Double

### 11.3.2.3 Length

The Length field is enabled only for attributes of the String data type. Note that using Application Builder, the maximum length that we can specify for a string attribute is 2000.

### 11.3.2.4 Repeating

In order to specify an attribute as multi-valued, the Repeating checkbox can be selected. If left unchecked, it signifies a single-valued attribute.

Fill in the other attribute fields shown under the General tab as per your specific requirements. In our case we had identified the cust_news_summary attribute to be mandatory (refer to the attribute design in figure 11.16) and hence we checked the checkbox against the field Attribute cannot be blank.

Under the Advanced tab (refer to figure 11.19), you can specify the search parameters used by client applications to search an object in a Docbase, based on its attribute values. By default, every attribute is searchable. You can negate this condition by unchecking the Searchable checkbox shown in the figure.

### 11.3.2.5 Default Search Value

You can provide some default search value that is always shown when using client applications for searching objects based on this particular attribute.

### 11.3.2.6 Allowable Search Operators

You can add/remove search operators that client applications will use for searching Docbase objects based on certain conditions for the attribute in question.

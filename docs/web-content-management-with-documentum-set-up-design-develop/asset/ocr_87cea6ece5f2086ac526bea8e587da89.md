It is best to prepare a small table to design the attributes for the custom object type and fill in the respective columns of the table as per your business-specific requirements. Let us discuss this in the context of our dummy website (please refer to figure 11.16):

| Attribute Name | Attribute Label | Data Type | Length | Single/Repeating | Mandatory | Values |
|---|---|---|---|---|---|---|
| cust_news_summary | News Article Summary | String | 200 | Single | Yes | None |
| cust_news_is_press_rel | Is this for press release? | Boolean | 0 | Single | No | None |
| cust_news_display_order | Display Order | Integer | 0 | Single | No | Conditional:
- 1,3,5,7 if News article is for press release
- 0,2,4,6 if News article is not for press release |
| cust_news_edit_approvers | Editors/App rovers | String | 100 | Repeating | No | None |

Figure 11.16: Attributes Design table

One should design the attributes very carefully, since after an attribute has been created and checked in to the Docbase, the following cannot be modified:

+   - Name of the attribute
+   - Data type of the attribute
+   - Whether the attribute is single or repeating
+   - Length of the attribute (can only be increased)

# 11.3 Adding Attributes to an Object Type

Choose the object type in question within the DocApp and select Insert | Attribute. We will first take an example by creating the attribute cust_news_summary for the object type cust_newsarticle.
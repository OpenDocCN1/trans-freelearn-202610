+   - **Field Group:** This allows you to group fields into logical sections. This is especially useful when your content types have a large number of fields.
+   - **Content Copy:** This allows you to duplicate content easily. This is useful if you want to add several items that are similar.
+   - **Content Permissions:** This gives you fine-grained control over which fields can be viewed and edited based on a user's role. This is especially important for sites on which several different types of users work.

We will enable the **Content**, **Number**, **Text**, and **Option Widgets** modules to aid us in creating our menu item type.

# Designing the menu item

*Good Eatin' Goal:* Design a content type to represent meals and drinks which are available at the restaurant.

*Additional modules needed:* CCK (http://drupal.org/project/cck).

# Basic steps

When you start designing your site it is a good idea to think about what types you will need, and design the types before building them. It is relatively easy to add new fields to a type, but if you do so you may need to modify all of the existing content, which can be extremely time-consuming.

Even seemingly simple types, such as an address, require some thought—depending on what addresses will be used. For example, if the addresses entered are only from the United States, typical fields are Street 1, Street 2, City, State, and Zip Code. If the address has to include international addresses, typical fields are Street 1, Street 2, City, State/Province, Zip/Postal Code, and Country. If the address is only for a single country, state, or city, you can eliminate or automatically fill out some of the fields, to make entering information more convenient for your editors and users. The types of validation that you do will also depend on what expected values can be entered. For example, for truly international sites, it is impractical to include every state or province in a drop-down list for users to select from. On the other hand, if you know that addresses are only within a specific country, giving a list of states or provinces is very easy.
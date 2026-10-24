# Configuring relations

In the *Joining layer data recipe*, we discussed that joins only append additional columns to existing features (1:1 or n:1 relationships). Using joins, it is, therefore, not possible to model 1:n relationships, such as "one zip code area containing n schools". These kinds of relationships can instead be modeled using relations. This recipe introduces the concept of relations and shows how you can put them to use.

# Getting ready

To follow this exercise, load zip code areas and schools from `zipcodes_wake.shp` and `schools_wake.shp`.

# How to do it...

Relations are configured in **Project Properties**. The dialog is very similar to the join dialog:

+   1. Define the two layers (**Referencing/Child** and **Referenced/Parent**), as well as the fields containing the common values/IDs. As you want to model "one zip code area contains n schools," the zip code dataset is the parent layer and the school dataset is the child layer. The connection between both datasets is established based on the zip code fields (**ADDRZIPCOD** and **ZIPNUM**), as shown in the following screenshot:

![](bbox=[0.313, 0.515, 0.688, 0.701])
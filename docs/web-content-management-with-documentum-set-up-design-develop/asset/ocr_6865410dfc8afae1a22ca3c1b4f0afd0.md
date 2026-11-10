The Object Type General tab is shown in figure 11.5:

![](bbox=[0.26, 0.175, 0.74, 0.52])

Figure 11.5: Object Type General tab

# 11.1.3 Supertype

Each object type can either extend from an existing object type in Documentum or have its supertype as NULL.

In our case, News Article is a document object type and so we extended it from dm_document object type. Other object types shown in the Super Type dropdown are: dm_job, dm_folder, dm_user, etc.

Each object type in Docbase is stored within its own database table in the Docbase's underlying RDBMS.

As a rule of thumb, do not extend the object type hierarchy too deep (deeper than three levels) since internally this amounts to as many database joins by Documentum. You could, for example, extend your custom object type cust_newsarticle from dm_document and further extend another custom object type (say cust_newsarticle_sub) from cust_newsarticle, but not deeper than this!

> It is better to decide the name and supertype of custom object type well, because after checking in the object type in the Docbase, you cannot change its name or its supertype.
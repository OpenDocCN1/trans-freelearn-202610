Clicking on a name opens the right panel in the **Database** view, and clicking on a table name opens the right panel in the **Table** view to browse this table. (see the *Right Panel* section for details.)

# Tree display of database names

A user might be allowed to work on a single database—for example **marc**.
Some system administrators offer a more flexible scheme by allowing user **marc** to create many databases, provided all have their names starting with **marc**, such as **marc_airline** and **marc_car**. In this situation, the left panel can be set to display a tree of these database names, like this:

![](bbox=[0.365, 0.32, 0.635, 0.515])

This feature is currently offered only in light mode, and is controlled by these parameters:

```
$cfg['LeftFrameDBTree'] = TRUE;
$cfg['LeftFrameDBSeparator'] = '_';
```

The default value of TRUE in $cfg['**LeftFrameDBTree**'] ensures that this feature is activated. A popular value for the separator is '_'.

# Full mode

The previous examples were shown in Light mode, but setting the $cfg['**LeftFrameLight**'] parameter to FALSE produces a complete layout of our databases and tables using collapsible menus (if supported by the browser):
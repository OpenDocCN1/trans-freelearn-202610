# 5
Changing Data

Data is not static, it changes often. This chapter focuses on editing and deleting data and its supporting structures – tables and databases.

# Edit mode

When we browse a table or view results from a search on any single-table query, small icons appear on the left or right of each table row as shown in the following screenshot:

![](bbox=[0.37, 0.49, 0.63, 0.56])

The row can be edited with the pencil-shaped icon and deleted with the X-shaped icon. The exact form and location of these controls are governed by:

```
$cfg['PropertiesIconic'] = TRUE;
$cfg['ModifyDeleteAtLeft'] = TRUE;
$cfg['ModifyDeleteAtRight'] = FALSE;
```

We can decide whether to display them on the left side, the right side, or on both sides. The $cfg['PropertiesIconic'] parameter can have the values TRUE, FALSE, or both. TRUE displays icons as seen in the previous image, FALSE displays Edit and Delete (or their translated equivalent) as links, and both displays the icon and the text.

The small checkbox beside each row is explained in the Multi-Row Edit and the Deleting Many Rows sections later in this chapter.
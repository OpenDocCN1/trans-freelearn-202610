# Ratings

The **Ratings** table is responsible for holding all the end user ratings. As you can see each rating ties back to the **SystemObjects** table and also directly to the record within the specified table. In addition to that, we are also tracking the option that was rated. The most important part of this table though is the **Score** field. The **Score** tracks the selected number of stars (in our case) that the end user has selected.

# System object rating options

In addition to the ratings table we also have the **SystemObjectRatingOptions** table. This table tracks all the options that are available for each of our system objects.

# Tags

The Tags table is responsible for holding all the textual tags that our users attach to the various objects in our system. Knowing that we intend to collect a number of the same tags for multiple objects in our system, it is important that we maintain a unique list of tags with counts rather than a single entry for every tag entered into the system and then having to sum up the tags to get a count. Performance-wise, maintaining the aggregate value as we collect tags will easily outperform determining the summed value each time we need the count.

# System object tags

This then brings us to our **SystemObjectTags** table. It is responsible for maintaining details of which tag is connected to which record of a certain type of object. This table simply maintains various record IDs to keep track of the relationships.

# Comments

This finally brings us to the Comments table. Of the five features, this is by far the easiest to implement as this table keeps track of everything for us directly. Each comment in this case is assumed to be unique and is therefore directly tied to an object and a record within that object. We also keep track of the user who left the comment (**CommentByAccountID**) and when (**CreateDate**) he/she did so. The current size of the comment can be up to 2,000 characters. However, if your community requires large comments, you can of course increase this number.
# DynamoDB Optimistic Locking

Optimistic locking is designed to prevent you from accidentally overwriting changes made by others and vice-versa. This feature requires each item to have an attribute in the table that can act as a version number. Updates to the item can only be successful if you can reference the correct (or latest) version number as a condition for the update.

For example, in the following diagram, without the use of a version number, note that Client B has been successful in updating the price of the product to $15, even though Client A performed an update just moments earlier to $17. While this may be an acceptable change, it could also be an error performed by Client B.

![](bbox=[0.13, 0.32, 0.86, 0.51])

Figure 6.18 – Database updates without optimistic locking

Notice in the preceding diagram, that Client B was able to update the table and set the price to $15. While in certain cases, this may be acceptable, it could also be an error. With optimistic locking, each item will need to have an attribute that can act as a version number. While retrieving an item from the table, the application would make a note of the version number. This allows the application on your client device to then update the item on the table provided that the version number doesn’t change in the interim. If there is a version number mismatch because another client was able to update the item, then your update will fail.
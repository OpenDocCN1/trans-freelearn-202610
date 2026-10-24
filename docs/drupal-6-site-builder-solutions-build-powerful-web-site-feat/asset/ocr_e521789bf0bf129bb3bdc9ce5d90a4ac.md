7. Add a relationship by clicking the + symbol next to the Relationships label. We will want the relationship as Node: Voting results as shown in the following screenshot:

Defaults: Add relationships
Groups:
Node
Node: Individual votes
Votes cast by users on node content.
Node: Voting results
The aggregate results of votes cast on node content.

8. After you have added the relationship, you will be prompted for more details of the relationship. We will need to set up this relationship so that any menu item that has not been rated will not be eligible. The other settings will control how the values are returned, as shown here:

Defaults: Configure Relationship "Node: Voting results"
Label:
Voting results
The label for this relationship that will be displayed only administratively.
Require this relationship
If required, items that do not contain this relationship will not appear.
Data filters
For each piece of content, many pieces of voting data may be saved. Use these options to specify exactly which types should be available via this relationship. Warning! Leaving any of these filters empty may result in multiple copies of each piece of content being displayed in listings.
Value type:
Points
Vote tag:
Default vote
Aggregation function:
Average
Update Cancel Remove

9. Next, add a couple of new fields to the View by clicking the + symbol next to the Fields label. Add the following fields:

+   - Node: Title with the label Title and a link to the node
+   - Voting API results: Function with the label Average Rating
When we are provided information about deletions, we distinguish two cases: when we get the full table payload, along with the information about what entities have been deleted, and when we are provided with only the keys of the deleted entities.

# Deletion with a full payload

The first case we discuss is when you receive all the normal columns with the payload for the table, plus another one that contains the state of the row (deleted VS active) or the operation that produces the row (creation, update, or deletion).

In this case, you can just consider the state/operation column like a normal payload column and load the table using the normal save_history macro, having that column as part of the HDIFF column of the table.

In this case, we suggest that, in the STG model, you add a Boolean DELETED column that encapsulates the specific rule to recognize if a row has been deleted or not. This helps in keeping the hard rules about the export and load encapsulated in the storage layer.

Handling deletion detection in this way is good because we are back to the normal case when we have only one source of truth for the content of the table.

One special case is when you have one initial or periodic reconciliation full exports without this extra state/operation column and more frequent delta loads that have this column instead.

The “trick” here is to consider that the full export provides you with rows that exist at some moment in time and that you can create the extra column in the STG model, all while designating such rows as active or with an operation code that could also have its own value.

In the next section we enter the realm of multiple sources of truth for the same table, so we need to establish a reliable timeline and eventually set some precedence rules in the STG model so that the right row for each entity is sent to the HIST table.

# Deletion with deleted keys only

When we do not have the full payload for the deleted rows and only their keys, we find ourselves in a situation close to the deletion from the full export, but with knowledge of what rows have been deleted without the need to have a full export in the input and without the need to perform an expensive join operation.

Our solution will load the entities coming from the input as usual, while we generate the rows to record the deletions using the history and the list of deletions.

Let’s discuss the code for this macro.
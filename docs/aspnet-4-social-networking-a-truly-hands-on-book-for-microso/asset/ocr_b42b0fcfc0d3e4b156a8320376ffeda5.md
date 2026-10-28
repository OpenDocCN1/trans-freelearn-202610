In order to process email that is received, we may have to go outside of what LINQ was designed for. For this reason, we will create a couple of stored procedures to help us out. We will have two procedures—one to essentially put the data from receiving into the working table, and the other to put the working data into the history table. This is just the idea, not the actual implementation!

Here is how we create the first procedure:

```
create procedure [dbo].[pr_MailQueue_SwapReceivingAndWorking_GetWorking]
as
begin tran
--rename working to temp
execute sp_rename
    @objname = 'MailQueue_Working',
    @newname = 'MailQueue_Temp'
--rename receiving to working
execute sp_rename
    @objname = 'MailQueue_Receiving',
    @newname = 'MailQueue_Working'
--rename temp to receiving
execute sp_rename
    @objname = 'MailQueue_Temp',
    @newname = 'MailQueue_Receiving'
select * from MailQueue_Working
commit tran
```

This procedure moves the data from one table to another in a manner that is more efficient than actually moving the data. We start by renaming the working table (an empty table at this point) to MailQueue_Temp. We then rename the receiving table (a full table) to MailQueue_Working. And finally, we rename the temp table to MailQueue_Receiving so that we can continue to collect new emails. With this complete, we then return all the records in the working table. If this process is not run frequently enough, you might find that there is more data in this table than you can process in a single chunk. For that reason, you either need to process it more often, or modify the selection to select pages of data. Our implementation will assume that you process your queue frequently enough so that the processor is never overloaded.

The next procedure, pr_MailQueue_MoveWorkingToHistory, cleans up after our email processor has completed its work.

```
create procedure [dbo].[pr_MailQueue_MoveWorkingToHistory]
as
begin tran
```
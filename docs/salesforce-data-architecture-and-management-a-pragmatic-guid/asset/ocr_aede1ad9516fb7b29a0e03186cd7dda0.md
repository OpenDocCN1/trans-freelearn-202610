One good reason for creating profiles is for page layout assignments that can only be controlled through profiles and not permission sets. Even in this case, I suggest taking a good look at your design and seeing whether there are other ways in which you can achieve your business requirements.

# # Removing unused fields and objects

This is a straightforward one but, surprisingly, I have seen organizations where the admin created an object with a few fields in production to *try out a quick **Proof of Concept** (POC)* and never got to remove the object completely once they were done. You should make a habit of not doing these types of POCs or enhancements in production in any case, but if you have to for any reason, maintain a list of fields and objects you are creating so that you can remove them if they are not required.

The same guideline applies to fields as well. Unused fields are no fun and cause confusion for end users in reporting, and especially if they are on the page layout, cause integration problems and just generally lead to a cluttered organization. Remove them if they are not used. Aim for no unused objects and fewer than 15% unused fields, which you need to put on your backlog for removal.

# # Report proliferation

Try to establish a robust framework on how reports will be managed. The ease with which reports can be created in Salesforce is a double-edged sword. It is easy for end users to create their own reports in a matter of minutes, but without any guidelines and governance, this can lead to report duplication, a plethora of unused reports, and reports with no description so no one knows what they do.

Run the following query to assess when a report was last run. For example, have a rule that says if a report has not been run in the last 18 months, mark it for deletion after discussing it with the report owner. You can have any time period you want, but I recommend 18 months because that would cover the calendar year start and end dates and non-calendar fiscal year dates as well. This would cover your bases in cases where a report is specifically run at certain times of the year:

```
SELECT Id, Name, Description, LastRunDate, LastModifiedBy.Name
FROM Report
WHERE LastRunDate < 2019-06-01T00:00:00Z
ORDER BY LastRunDate DESC
LIMIT 50
```
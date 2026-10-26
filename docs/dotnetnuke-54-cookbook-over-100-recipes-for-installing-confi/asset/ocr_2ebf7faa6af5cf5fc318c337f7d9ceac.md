```
' build the strings with the fields we want to index
strTitle = ModInfo.ModuleTitle + " - " + objEmployee.ContactInfo()
strDescription = objEmployee.EmpFirstName & " " & objEmployee.EmpLastName & " - Hired " & objEmployee.FormattedHireDate & " New Hire: " & objEmployee.NewHire & " Email:" & objEmployee.EmailAddress
intAuthor = objEmployee.CreatedByUser
dtePubDate = objEmployee.CreatedDate
strSearchKey = objEmployee.ItemId.ToString
strContent = System.Web.HttpUtility.HtmlDecode(objEmployee.EmpFirstName & " " & objEmployee.EmpLastName & " " & objEmployee.Salary.ToString & " " & objEmployee.HireDate.ToString & " " & objEmployee.EmailAddress)

' Create a search item and pass the strings as arguments
Dim SearchItem As SearchItemInfo = New SearchItemInfo(strTitle, strDescription, intAuthor, dtePubDate, ModInfo.ModuleID, strSearchKey, strContent, "ItemId=" & objEmployee.ItemId.ToString)
SearchItemCollection.Add(SearchItem)

Next

Return SearchItemCollection

End Function
```

+   5. Now that we have defined what is searchable in the module, rebuild and redeploy the module.
+   6. The next step is to index the module data to make it searchable. Log in as SuperUser.
+   7. Select **Search Admin** from the **Host** menu.

![](bbox=[0.21, 0.74, 0.78, 0.88])
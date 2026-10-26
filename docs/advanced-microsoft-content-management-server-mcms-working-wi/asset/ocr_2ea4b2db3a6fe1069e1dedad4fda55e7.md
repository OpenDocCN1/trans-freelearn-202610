> The PAPI does not provide a method for creating resource galleries. Resource galleries can only be created by using Site Manager.

Let's build a Create Resource dialog for the CMS Explorer:

+   1. Add a new web form to the CMSExplorer project. Name the new web form CreateResource.aspx.
+   2. In Design view, drag and drop styles.css onto the web form.
+   3. Toggle to HTML view and add a table with six rows as shown below:
+   4. With the CreateResource.aspx web form open in Design view, add controls (from the Web Forms section of the Toolbox) to the form and set them up as shown below.

```
<table>
<tr>
<td colspan="2">
<h1>Create Resource</h1>
<h2>
Parent Resource Gallery:
(Add Literal for displaying the path here)
</h2>
</td>
</tr>
<tr>
<td>Display Name:</td>
<td>(Add the text box for the display name here)</td>
</tr>
<tr>
<td>Description:</td>
<td>(Add the text box for the description here)</td>
</tr>
<tr>
<td>File:</td>
<td><INPUT type="file" id="File1" runat="server"></td>
</tr>
<tr>
<td colspan="2">(Add the label for error messages here)</td>
</tr>
<tr>
<td colspan="2" align="right">
(Add the Create Resource button here)
<INPUT type="button" value="Close" onclick="javascript:window.close();">
</td>
</tr>
</table>
```

| Control | Property | Value |
|---|---|---|
| Literal | ID | litParentGallery |
| TextBox | ID | txtDisplayName |
| TextBox | ID | txtDescription |
|  | Rows | 3 |
|  | TextMode | MultiLine |
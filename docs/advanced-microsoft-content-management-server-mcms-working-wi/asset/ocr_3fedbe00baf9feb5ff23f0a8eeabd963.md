```
row.Cells.Add(cell);
// return the built row
return row;
}
```

Let's see how our placeholder control looks so far. Build the TropicalGreenControlLib project. Assuming there were no errors, let's now add our placeholder control to the template. Instead of adding it to an existing template, create a new template file in the TropicalGreen project (under the Templates subdirectory) and name it Events.aspx. Use the Homepage.aspx template as a base for the Events.aspx page, but remove the content area so that your final Events template page's HTML contains the following (note the highlighted portions where we have added our new placeholder control):

```
<%@ Page language="c#" Codebehind="Events.aspx.cs" AutoEventWireup="false" Inherits="TropicalGreen.Templates.Events" %>
<%@ Register TagPrefix="uc1" TagName="DefaultConsole" Src="../Console/DefaultConsole.ascx" %>
<%@ Register TagPrefix="cc1" Namespace="TropicalGreenControlLib" Assembly="TropicalGreenControlLib" %>
<%@ Register TagPrefix="uc1" TagName="RightMenu" Src="../UserControls/RightMenu.ascx" %>
<%@ Register TagPrefix="uc1" TagName="TopMenu" Src="../UserControls/TopMenu.ascx" %>
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN" >
<HTML>
<HEAD>
<title>Events</title>
<meta content="Microsoft Visual Studio .NET 7.1" name="GENERATOR">
<meta content="C#" name="CODE_LANGUAGE">
<meta content="JavaScript" name="vs_defaultClientScript">
<meta content="http://schemas.microsoft.com/intellisense/ie5" name="vs_targetSchema">
<LINK href="/tropicalgreen/Styles/Styles.css" type="text/css" rel="stylesheet">
</HEAD>
<body leftMargin="0" topMargin="0">
<form id="Form1" method="post" runat="server">
<table cellSpacing="0" cellPadding="0" width="100%" border="0">
<tr>
<td vAlign="top" width="100%" bgColor="#ffcc00" colSpan="2">
<IMG src="/tropicalgreen/images/Logo.gif">
</td>
<td vAlign="top" rowSpan="10">
<uc1:DefaultConsole id="DefaultConsole1" runat="server"></uc1:DefaultConsole>
</td>
</tr>
<tr bgColor="#66cc33">
<td colSpan="2">
<uc1:topmenu id="TopMenu1" runat="server"></uc1:topmenu>
</td>
</tr>
<tr>
<td style="PADDING-RIGHT: 30px; PADDING-LEFT: 30px; PADDING-BOTTOM: 30px; PADDING-TOP: 10px" vAlign="top">
<p>&nbsp;</p>
<b>Tropical Green Events</b>
<cc1:SharePointEventsPlaceholderControl id="SharePointEvents" runat="server" PlaceholderToBind="SharePointEventsConfig" />
</td>
<td class="RightMenuBar" vAlign="top"
```
10. To position the images, we start with an encompassing table to set the style of the container, then include a table for the top of the container, a place for the ContentPane, then another table to hold the images for the bottom of the container:

```
<table width="100%" border="0" align="center" cellpadding="0" cellspacing="0" class="ASCXImage_Master">
<tr>
<td class="ontainerUC2">
<table height="27" border="0" cellpadding="0" cellspacing="0">
<tr>
<td valign="top" class="ContainerUL">
<dnn:ACTIONS runat="server" id="dnnACTIONS" />
</td>
<td valign="top" class="ContainerUL">
<dnn:ICON runat="server" id="dnnICON" />
</td>
<td valign="top" class="ContainerUL">&nbsp;
<dnn:TITLE runat="server" id="dnnTITLE" CssClass="ASCXImage_Title"/>
</td>
<td valign="top" class="ContainerUC1">&nbsp;</td>
<td valign="bottom" class="ContainerUC2">&nbsp;</td>
</TR>
</table>
<table width="100%" border="0" align="center" cellPadding="0" cellSpacing="0">
<tr>
<td class="ASCXImage_Content">
<div runat="server" id="ContentPane">
</div>
</td>
</TR>
</table>
<table width="100%" height="21" border="0" cellpadding="0" cellspacing="0">
<tr>
<td valign="middle" class="ContainerLL">&nbsp;</td>
<td valign="middle" class="ContainerLR">&nbsp;</td>
</tr>
</table>
</td>
</tr>
</table>
```
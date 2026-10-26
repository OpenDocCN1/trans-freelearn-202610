# Basic Skinning

```
Src="~/Admin/Skins/Login.ascx" %>

<div >
    <dnn:NAV runat="server" id="dnnNAV"
             ProviderName="DNNMenuNavigationProvider"
             IndicateChildrens="false"
             ControlOrientation="Horizontal" />
</div>

<div id="login_style" class="user">
    <dnn:USER runat="server" id="dnnUSER" CssClass="user" />
    &nbsp;&nbsp;|&nbsp;&nbsp;
    <dnn:LOGIN runat="server" id="dnnLOGIN" CssClass="user" />
</div>

<div class="content">
    <table width="100%" border="0" cellspacing="0" cellpadding="0">
        <tr>
            <td valign="top" id="LeftPane" class="LeftPane"
                runat="server" visible="false">
            </td>
            <td valign="top" id="ContentPane" class="ContentPane"
                runat="server" visible="false">
            </td>
            <td valign="top" id="RightPane" class="RightPane"
                runat="server" visible="false">
            </td>
        </tr>
    </table>
</div>
```

+   12. Select **Save WebUserControl1.ascx As...** from the **File** menu.
+   13. Browse to the `SampleASCXSkin` folder and save the file as `SampleASCXSkin.ascx`.
+   14. To create the CSS file select **New File...** from the **File** menu.
+   15. In the **New File** dialog, select **Style Sheet** and click on **Open**.
+   16. Replace the contents of the file with:

```
/* General Styles */
body                         {background: #CCCCCC; height:
100%; margin: 0px; padding: 0px;}
a, a:link, a:visited, a:active  {color: #800000; text-decoration:
none; font-family: Verdana, Arial, Helvetica, sans-serif; font-
weight: normal;}
```
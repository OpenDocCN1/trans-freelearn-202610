```
```aspx
<dnn:Label ID="lblSalaryRange" runat="server"
           ControlName="ddlddlSalaryRange" Suffix=":">
</dnn:Label>
</td>
<td width="18%">
    <asp:DropDownList ID="ddlSalaryRange" runat="server"
                      DataTextField="Text" DataValueField="Value"
                      AutoPostBack="true">
    </asp:DropDownList>
</td>
<td width="5%">
    &nbsp;
</td>
<td width="12%" class="SubHead">
    <dnn:Label ID="plRecordsPage" runat="server"
               CssClass="SubHead"
               ResourceKey="Recordsperpage" Suffix=":">
    </dnn:Label>
</td>

<td width="18%">
    <asp:DropDownList ID="ddlRecordsPerPage"
                      runat="server" AutoPostBack="True">
        <asp:ListItem Value="10">10</asp:ListItem>
        <asp:ListItem Value="25">25</asp:ListItem>
        <asp:ListItem Value="50">50</asp:ListItem>
        <asp:ListItem Value="100">100</asp:ListItem>
        <asp:ListItem Value="250">250</asp:ListItem>
    </asp:DropDownList>
</td>

</tr>
</table>
```
```

+   4. Next, scroll down to the bottom of the file and add this paging control code after the Datagrid:
+   
+   ```aspx
+   <br />
+   <dnn:PagingControl ID="ctlPagingControl" runat="server">
+   </dnn:PagingControl>
+   <br />
+   ```

+   5. Double-click to open the ViewEmployee.ascx.vb file.
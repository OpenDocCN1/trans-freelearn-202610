+   6. Next, open the EditEmployee.ascx.vb file.

+   7. Check the top the file and make sure it has the following imports:
+   Imports DotNetNuke.UI.WebControls
+   Imports DotNetNuke.Common.Lists

+   8. Scroll down to the Private Methods region and add this procedure:
+   #Region "Private Methods"
+   
+   Private Sub SuggestText(ByVal objNodes As DNNNodeCollection, ByVal strTypedText As String)
+   
+       Dim nodeStates As DNNNode
+       Dim objList As New ListController()
+       Dim lstStates As ListEntryInfoCollection = objList.GetListEntryInfoCollection("Region", "Country.US")
+   
+       Dim state As ListEntryInfo
+   
+       'loop through the collection and add it to the result set
+   
+       For Each state In lstStates
+           If txtState.MaxSuggestRows = 0 Or objNodes.Count < (txtState.MaxSuggestRows + 1) Then
+               ' add the state to the list of suggestions
+               nodeStates = New DNNNode(state.Text)
+               nodeStates.ID = state.Value
+               objNodes.Add(nodeStates)
+           End If
+       Next
+   
+   End Sub

+   9. Next, we must trap when text is typed into the control. Scroll down to the bottom of the Event Handlers region and add the following procedure:
+   Private Sub txtState_UpdateLabel(ByVal sender As Object, ByVal e As DotNetNuke.UI.WebControls.DNNTextSuggestEventArgs) Handles txtState.PopulateOnDemand
+   
+       ' call the suggestion procedure and pass the text typed into the control
+       SuggestText(e.Nodes, e.Text)
+   
+   End Sub
+   
+   #End Region
the dataset using a standard configuration or script. The script is similar to what you saw for the JDBC data source, and for this reason, we will concentrate the discussion on the regular settings.

The first item in the panel (at the top) relates to the definition of an XMI file. As an example, we used the metadata.xmi file bundled in the Pentaho Report Designer distribution. In the preceding screenshot, you can check the exact location of the file (in the report-designer/samples folder).

In the Domain Id field, specify the name of the solution directory this metadata file pertains to. In the Available Queries panel, you can see the list of all the queries defined for the current data source. As you saw for the JDBC queries, you can use the green plus icon (⊕) to add a new query and you can use the red icon (⊗) to delete an existing query.

In the Query Name field, you can enter a meaningful label to identify the query, and at the bottom of the window, you have the query content. Like you saw for the JDBC data source, the query content can be defined using a static query or a script. The script is the same as described for the JDBC data source and the static query is in XML format.

In consideration of the XML format of the query being less easy for users, a metadata Query Editor is provided. To access the metadata Query Editor, click on the small pencil icon (✎) in the top right-corner of the panel. Once clicked, a window similar to the following one is shown:

![](bbox=[0.11, 0.62, 0.68, 0.89])
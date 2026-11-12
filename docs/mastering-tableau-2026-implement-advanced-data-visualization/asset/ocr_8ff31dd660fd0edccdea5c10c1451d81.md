You can supply JSON files to define credentials for input or output data connections, making this suitable for automated pipelines:

```
tableau-prep-cli run myflow.tfl --connection-credentials credentials.json
```

The credentials.json file would contain something like:

```
{
  "connections": [
    {
      "id": "input-connection-id",
      "username": "your_username",
      "password": "your_password"
    }
  ]
}
```

What you need to know:

+   - The tableau-prep-cli tool installs with Tableau Prep Builder
+   - You can run flows on any machine that has Prep Builder installed
+   - While scheduling requires the Data Management add-on on Tableau Server, command-line execution gives you flexibility for custom automation

It’s not for everyone, but for those who need it, this feature is a lifesaver.

# Exporting data

Last, but not least, we may want to export our data. We have seen before that a right-click on a symbol in the flow pane offers the possibility to preview the data in Tableau Desktop:

+   1. If you want a flow to run according to a schedule or save it to a location, it’s best to use **Add | Output.**
+   2. After you do this, the following screen will appear:
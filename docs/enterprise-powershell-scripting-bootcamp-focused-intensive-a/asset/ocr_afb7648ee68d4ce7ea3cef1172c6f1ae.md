There is no output to display from creating this code.

In this example, you create the search extension array including a wildcard:

+   1. To start, you declare $srextlist as an array.
+   2. You then leverage the read-xmltag function with the srext argument to gather the search extensions from the XML file in memory.
+   3. You pipe the result to %, or the alias of foreach, where you append an * to the beginning of the current extension, which is in the $_.id XML property.
+   4. After execution, you will have an array of wildcard extensions in the $srextlist.

The last part of this section is creating the regular expressions from the XML data in memory. To create multiple regular expressions from the XML data, you perform the following:

```
# File Search Data
$srterms = read-xmltag "srterm" | % { decrypt-string $_.id }
$srtermsRegEx = create-SearchRegEx $srterms
```

```
# File Ignore list
$flign = (read-xmltag "flign") | Select id
$flignRegEx = create-SearchRegEx $flign.id
```

```
# Search Ignore List
$srign = (read-xmltag "srign") | Select id
$sringRegEx = create-SearchRegEx $srign.id
```

```
# Built-in User List
$blst = (read-xmltag "blst") | Select id
$blstRegEx = create-SearchRegEx $blst.id
```
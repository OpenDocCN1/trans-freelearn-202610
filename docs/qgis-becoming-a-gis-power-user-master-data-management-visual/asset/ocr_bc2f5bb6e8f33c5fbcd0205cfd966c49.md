# Parsing the JSON data

To work with the JSON data from this web service, we will use jQuery's AJAX capabilities. Using the attributes of the JSON elements, we can take a look at how the data is rendered in HTML as a simple web page.

# Starting up the server

Start up SimpleHTTPServer on port 8000 for c7\data\web on the Windows command line using the following commands:

```
```
cd c:\packt\c7\data\web
python -m SimpleHTTPServer 8000
```
```

# Test parsing with jQuery

You can take a look at the following code (on the file system at c7\data\web\gsheet.html) to test our ability to parse the JSON data:

```
```
<html>
<body>
<div class="results"></div>
</body>
<script src="http://code.jquery.com/jquery-1.11.3.min.js">
</script>
<script>
// ID of the Google Spreadsheet
var spreadsheetID = "1xAc8wpgLgT2zpvZmZau2oiOIuhA_31ojKSI8miL6FM6Mzq";
// Make sure it is public or set to Anyone with link can view
var url = "https://spreadsheets.google.com/feeds/list/" + spreadsheetID + "/1/public/values?alt=json";
$.getJSON(url, function(data) {
var entry = data.feed.entry;
$(entry).each(function() {
// Column names are name, type, etc.
$('.results').prepend('<h2>'+$this.gs$x$name.$t+'</h2><p>'+$this.gs$x$type.$t+'</p>');
});
});
</script>
```
```
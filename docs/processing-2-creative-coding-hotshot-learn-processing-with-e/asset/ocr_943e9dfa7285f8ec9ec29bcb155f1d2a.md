3. In the setup() method, we read the file into an array of strings.

```
void setup() {
size(700,350);
String[] log = loadStrings( "access.log" );
}
```

4. Add a new tab by clicking on the little arrow icon and selecting New Tab. We name it LogRow and create a new class to store the IP address and the timestamp of each record.

```
class LogRow {
String ip;
String timestamp;

public LogRow( String ip, String timestamp ) {
this.ip = ip;
this.timestamp = timestamp;
}
}
```

5. Switch back to our our main sketch and add a method named parseLogfile(), which takes a string array as a parameter and returns an array of the LogRow classes.

```
void draw() {
}

LogRow[] parseLogfile( String[] rows ) {
}
```

6. In the parseLogfile() method, we iterate over the strings and use the match() method to extract the IP address and the date part. This function uses a regular expression to filter the text and returns an array of strings. The array contains a string for each pair of brackets in the regular expression. Our regular expression extracts IP addresses in the form xxx.xxx.xxx.xxx followed by two dashes and a date in squared brackets. A detailed description of how to write a regular expression can be found in the Processing reference documentation.

```
LogRow[] parseLogfile( String[] rows ) {
for( int i=0; i< rows.length; i++ ) {
String[] m = match(
rows[i], "\\d+.\\d+.\\d+.\\d+ - \\\[(.*)\\)";
if ( m != null ) {
println( m[1] + " " + m[2] );
}
}
}
```
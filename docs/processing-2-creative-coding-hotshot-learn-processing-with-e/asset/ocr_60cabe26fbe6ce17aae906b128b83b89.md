+   2. Download the hip_ip4_city_lat_lng.csv file from the cvs folder.

+   3. Now, open the sketch we created for the previous task and add the file by dragging it onto the sketch window or by using the Sketch | Add File... menu.

+   4. Create a new tab and name it IPdb. We add a new class to store the IP address blocks and its latitude/longitude pair.

```
public class IPdb{
long ipblock;
String lat;
String lon;

public IPdb( long ipblock, String lat, String lon ) {
this.ipblock = ipblock;
this.lat = lat;
this.lon = lon;
}
}
```

+   5. To load the database, we create a new method that parses the .csv file and stores the records in our IPdb class. We create a HashMap data structure with the IP block as an index so we can access it faster when we geocode the logfile rows. HashMap is a Java data structure that maps a unique key to a different object and allows us to retrieve the stored object using the key.

```
HashMap<Long, IPdb> parseIpDatabase( String filename ) {
String[] raw = loadStrings( filename );

HashMap<Long, IPdb> map= new HashMap<Long, IPdb>(total);
for( int i=0; i<raw.length; i++ ) {
String[] parts = raw[i].split( ",");
int ipblock = int( parts[0] );
map.put( new Long(ipblock), new IPdb( ipblock, parts[2],
parts[3] ));
}
return map;
}
```

+   6. In our setup() method, we add a call to the parseIpDatabase() method and HashMap for our database.

```
import java.text.SimpleDateFormat;

LogRow[] data;
HashMap<Long, IPdb> ipdatabase;

void setup() {
```
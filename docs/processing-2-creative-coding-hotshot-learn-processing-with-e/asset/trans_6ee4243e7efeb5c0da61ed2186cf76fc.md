

size(700,350);

ipdatabase = parseIPDatabase( "hip_ip4_city_lat_lng.csv" );
String[] log = loadStrings( "access.log" );
data = parseLogfile( log );
}

- 7. 现在，我们可以通过搜索表来查找任何给定 IP 地址的地理坐标，但首先我们需要扩展 LogRow 类来存储坐标。切换到我们草图的 LogRow 选项卡并添加以下属性：

```
class LogRow {
String ip;
long timestamp;
String lat;
String lon;

public LogRow( String ip, long timestamp ) {
this.ip = ip;
this.timestamp = timestamp;
}

void setLatLon( String lat, String lon ) {
this.lat = lat;
this.lon = lon;
}
}
```

- 8. 现在，回到主草图，我们添加另一个名为 geoCode() 的方法，它将记录数组作为参数。

```
void geoCode( LogRow[] data ) {
}
```

- 9. 在我们的方法中，我们遍历所有行并尝试查找 IP 地址的地理坐标。如果无法找到纬度和经度，我们将它们设置为 null。数据库不使用实际 IP 地址作为键，而是使用 IP 块的数字表示。要将 IP 地址转换为这个搜索键，我们需要通过在点号处拆分 IP 地址并将数字乘以 256 的幂来将其转换为数值。

```
void geoCode( LogRow[] data ) {
for ( int i=0; i < data.length; i++ ) {
String[] block = data[i].ip.split( "\\." );
int ip = int( block[0] ) * 256 * 256 * 256 + int( block[1] ) * 256 * 256 + int( block[2] ) * 256;
IPdb r = ipdatabase.get(ip);
if ( r != null ) {
data[i].setLatLon( r.lat, r.lon );
}
}
}
```

- 10. 我们的草图在 setup() 方法中解析日志文件并对 IP 地址进行地理编码，根据日志文件的大小，这可能需要一段时间。为了告知用户启动为什么异常缓慢，我们现在将解析移到后台线程中运行。

```
void setup() {
size(700,350);

Thread t = new Thread() {
public void run() {
ipdatabase = parseIPDatabase( "hip_ipv4_city_lat_lng.csv" );
String[] log = loadStrings( "access.log" );
data = parseLogfile( log );
geoCode( data );
}
};

t.start();
}
```

- 11. 现在，setup() 方法会立即返回，但在解析完成之前，draw() 方法中将没有可用的行。因此，我们需要修改 draw() 方法并为用户打印一条消息，以告知他们正在发生什么。

```
void draw() {
background(0);

if ( data == null ) {
text( "Loading ...", 35, 190 );
}
else {
noStroke();
fill(255);
rect(0, 150, 700, 50);
stroke(0, 10);
long mints = data[0].timestamp;
long maxts = data[ data.length-1].timestamp;

for ( int i=0; i<data.length; i++) {
float pos = map( data[i].timestamp, mints, maxts, 0, 700 );
line( pos, 150, pos, 200 );
}
}
}
```

- 12. 这好多了，但用户仍然不知道解析过程需要多长时间，因此让我们添加一个进度条，指示解析或地理编码已经完成了多少。我们需要添加两个变量来保存总行数和已处理记录数。我们还添加一个状态变量，存储当前所处的解析阶段。

```
int LOADING = 0;
int LOGFILE = 1;
int GEOCODING = 2;
int DONE = 3;

int state = LOADING;

int count = 0;
int total = 0;

HashMap<Long, IPdb> ipdatabase;
LogRow data[];

void setup() {
    size(700, 350);

    Thread t = new Thread() {
        public void run() {
            state = LOADING;
            ipdatabase = parseIpDatabase( "hip_ip4_city_lat_lng.csv" );
            state = LOGFILE;
            String[] log = loadStrings( "access.log" );
            data = parseLogfile( log );
            state = GEOCODING;
            geoCode( data );
            state = DONE;
        }
    };
    t.start();
}
```

- 13. 在我们的 parseIpDatabase() 方法中，我们将 total 变量设置为数据库中的行数，并在 for 循环中更新 count 变量。

```
HashMap<Long, IPdb> parseIpDatabase( String filename ) {
    String[] raw = loadStrings( filename );
    total = raw.length;
    HashMap<Long, IPdb> map= new HashMap<Long, IPdb>(total);
    for ( int i=0; i<raw.length; i++ ) {
        count = i;
        String[] parts = raw[i].split( "," );
        int ipblock = int( parts[0] );
        map.put( new Long(ipblock), new IPdb( ipblock, parts[2],
                parts[3] ) );
    }
    return map;
}
```

- 14. 在我们的 parseLogfile() 方法中，我们需要将 total 变量设置为日志文件中的行数，并在循环中更新 count 变量。

```
LogRow[] parseLogfile( String[] rows ) {
    total = rows.length;
    SimpleDateFormat sdf = new SimpleDateFormat( "dd/MMM/YYYY:HH:mm:ss Z" );
    LogRow[] res = new LogRow[ rows.length ];
    for ( int i=0; i<rows.length; i++ ) {
        count = i;
        String[] m = match(
            rows[i], "\\\\(\\d+\\.\\d+\\.\\d+\\.\\d+\\\\) - \\\\[(.*)\\\]" );
        if ( m != null ) {
            try {
                long time = sdf.parse( m[2] ).getTime();
                res[i] = new LogRow( m[1], time );
            }
            catch( Exception e ) {
                e.printStackTrace();
            }
        }
    }
    return res;
}
```

- 15. 最后，我们需要更新 geoCode() 方法，将 total 变量设置为已解析的行数，并将 count 变量设置为循环计数。

```
void geoCode( LogRow[] data ) {
    total = data.length;
    for ( int i=0; i < data.length; i++ ) {
        count = i;
        String[] block = data[i].ip.split( "\\." );
        int ip = int( block[0] ) * 256 * 256 * 256 + int( block[1] ) * 256 * 256 + int( block[2] ) * 256;
        IPdb r = ipdatabase.get(ip);
        if ( r != null ) {
            data[i].setLatLon( r.lat, r.lon);
        }
    }
}
```

- 16. 现在我们可以在 draw() 方法中绘制进度条并显示阶段名称。

```
void draw() {
    background(0);

    if ( state < DONE ) {
        stroke( 0, 200 );
        strokeWeight(2);
        fill( 200, 200 );
        rect( 25, 150, 650, 50 );
        noStroke();
        fill( 255 );
        rect( 30, 155, map( count, 0, total, 0, 640 ), 40 );
    }
    else if (state == DONE) {
        noStroke();
        fill(255);
        rect(0, 150, 700, 50);
        stroke(0, 10);
        long mints = data[0].timestamp;
        long maxts = data[data.length-1].timestamp;
        for (int i=0; i<data.length; i++) {
            float pos = map(data[i].timestamp, mints, maxts, 0, 700);
            line(pos, 150, pos, 200);
        }
    }

    fill( 0 );
    String msg="";
    if ( state == LOADING ) {
        msg = "loading database ...";
    }
    else if ( state == LOGFILE ) {
        msg = "parsing logfile ...";
    }
    else if ( state == GEOCODING ) {
        msg = "geocoding ip adresses ...";
    }

    text( msg, 35, 190 );
}
```

- 17. 当草图启动时，将显示如下屏幕截图中的进度条：

![](img/5cb67baa7bc7778de360b14c00d1ab81_213_0.png)

## 目标完成 - 小结

我们当前任务的目标是确定每个 IP 地址的地理坐标，并将其存储在 IpDB 类中。我们下载并安装了 hostIP 数据库的 .csv 版本，并将其添加到我们的草图中。我们创建了一个类来存储 IP 地址块的地理坐标，并创建了 parseIpDatabase() 命令来创建一个包含 hostIP 数据库的 HashMap。然后，我们编写了一个名为 geocodeIp() 的新方法，它接受一组 RowLine 对象，如果可以找到 IP 地址，则用纬度和经度丰富它们，否则设置为 null。

由于 setup() 方法中的加载和解析会减慢草图的启动速度，我们将日志文件处理和地理编码移至单独的线程并在后台运行。我们还在 draw() 方法中添加了一个进度条，以向用户显示已加载多少数据库或已处理多少日志文件。

## 红点热

每个 GIS 开发者做的首批应用之一就是在地图上绘制红点。这是地理信息系统的“Hello World”等价物，也是我们当前任务要做的。我们将使用作为霓虹地球贴图的世界地图，并为每个日志文件条目在地图上显示红点。


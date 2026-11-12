
To make our object more interesting, we added four additional control parameters to our GUI and extended the getR() function to take the height of the cylinder into account for calculating the radius.

## Exporting the object

We have created a 3D object in our previous task that can be changed using some sliders, but to print the object on a 3D printer, we have to save the object in a format that a 3D printer understands. Our current task is to add an export function that saves the object to an STL (STereo Lithography) file. This file format was originally invented to serve as an input format for Stereo Lithograph machines (hence the name), but nearly every currently available 3D software can import these files. The file stores the coordinates of the triangles and a normal vector for each of these triangles. This is why the format has the nickname Triangle Soup.

An STL file has an 80-byte header that is ignored by every known 3D program, so we set it to 0. Then, we need to write the count of the triangles as a four-byte unsigned long. This is followed by 12 float values for each triangle, which contain the normal vector and the coordinates of the vertices. Each triangle ends with a two-byte zero.

## Engage Thrusters

Let's export some triangles:

- 1. First, we add a class where we can store the triangles that our mesh is made of.
- Add a new tab by clicking on the icon on the right and select the New Tab ... menu.
- Name it Triangle and add the following code:

```
public class Triangle {
  PVector a;
  PVector b;
  PVector c;
  PVector n;

  public Triangle( PVector a, PVector b, PVector c, PVector n ) {
    this.a = a;
    this.b = b;
    this.c = c;
    this.n = n;
  }
}
```

- 2. Now we add another tab and create a class named TriangleSoup. This class stores the triangles and writes them to the file in the STL format.

```
import java.io.FileOutputStream;
import java.nio.ByteBuffer;
import java.nio.ByteOrder;
import java.util.Arrays;
import java.io.FileOutputStream;
import java.io.IOException;
import java.util.List;

public class TriangleSoup {

	List<Triangle> triangles = new ArrayList<Triangle>();
	float scale = 1;

	void scale( float scale ) {
		this.scale = scale;
	}

	void add( PVector a, PVector b, PVector c, PVector n ) {
		triangles.add( new Triangle( a, b, c, n ) );
	}
}
```

- 3. When all the triangle objects of our mesh have been added to our Triangle Soup, we need to save it to an STL file. Add the following method to our class:

```
void save( String filename ) {
try {
	byte[] header = new byte[80];
	byte[] count = new byte[4];
	byte[] tri = new byte[50];

	FileOutputStream out = new FileOutputStream( filename );
	ByteBuffer buffer = ByteBuffer.allocate( 80 );
	buffer.order( ByteOrder.LITTLE_ENDIAN );

	Arrays.fill(header, 0, 80, (byte)0);
	out.write( header, 0, 80 );
	buffer.putInt( triangles.size() );
	buffer.rewind();
	buffer.get( count, 0, 4 );
	out.write( count, 0, 4 );

	for( int i=0; i<triangles.size(); i++) {
		buffer.rewind();
		buffer.putFloat( triangles.get(i).n.x * scale );
		buffer.putFloat( triangles.get(i).n.z * scale );
		buffer.putFloat( triangles.get(i).n.y * scale );
		buffer.putFloat( triangles.get(i).a.x * scale );
		buffer.putFloat( triangles.get(i).a.z * scale );
		buffer.putFloat( triangles.get(i).a.y * scale );
		buffer.putFloat( triangles.get(i).b.x * scale );
		buffer.putFloat( triangles.get(i).b.z * scale );
		buffer.putFloat( triangles.get(i).b.y * scale );
		buffer.putFloat( triangles.get(i).c.x * scale );
		buffer.putFloat( triangles.get(i).c.z * scale );
		buffer.putFloat( triangles.get(i).c.y * scale );
		buffer.putShort( (short)0 );
		buffer.rewind();
		buffer.get( tri, 0, 50 );
		out.write( tri, 0, 50 );
	}
	out.flush();
	out.close();
} catch ( IOException ioe ) {
	ioe.printStackTrace();
}
}
```

- 4. Back on our main tab, we need to add a new button to our menu that opens a file selection box and lets the user enter the filename for the export file. So, we will now add a new button definition to our setupGUI() method.

```
void setupGUI() {
    Button exportButton = cp5.addButton( "export", 1, 10, 165, 120, 15 );
    exportButton.setLabel( "export" );
    exportButton.moveTo( box );
}
```

- 5. Now add a callback function for the export button and create a file dialog using the `selectOutput()` method. The first parameter is the title of the dialog box, and the second parameter is the name of a callback method that gets activated when the user selects a filename and clicks on OK.

```
void export() {
    selectOutput( "Export to STL", "exportCallback" );
}
```

- 6. The callback method for the dialog box gets the filename that the user selects, as a string parameter. If the user cancels the dialog, the filename string is set to null. If we get a valid filename, we convert it to an absolute path and create a TriangleSoup object. To make our object printable, we need to make sure that it is a closed volume and not a single wall. We add the `exportOuter()` and `exportInner()` methods, which create the walls, and the `exportBottom()` and `exportTop()` methods, which close our object.

```
void exportCallback(File exportFile) {
    if ( exportFile != null ) {
    String filename = exportFile.getAbsolutePath();
    TriangleSoup export = new TriangleSoup();
    export.scale( .5 );

    exportOuter( export );
    exportInner( export );
    exportBottom( export );
    exportTop( export );
    export.save(filename);
    }
}
```

- 7. Now we need to add the export functions for each of the meshes. The `exportOuter()` method creates triangles for the outer walls and then calculates the normal vectors for the triangles.

```
void exportOuter( TriangleSoup export ) {
    for ( int h = 1; h < 20; h++ ) {
    for ( int a = 0; a<72; a++ ) {
    int ab = (a+1) % 72;
    PVector p1 = new PVector( vertx[h][a], 95-h*5.0, verty[h][a] );

    PVector p2 = new PVector( vertx[h-1][a], 95-(h-1)*5.0, verty[h-1][a] );
    PVector p3 = new PVector( vertx[h][ab], 95-h*5.0, verty[h][ab] );
    PVector p4 = new PVector( vertx[h-1][ab], 95-(h-1)*5.0, verty[h-1][ab] );

    PVector n1 = new PVector( - (p1.x+p2.x+p3.x)/3, 0, - (p1.y+p2.y+y+p3.y)/3 );
    n1.normalize();
    PVector n2 = new PVector( - (p2.x+p3.x+p4.x)/3, 0, - (p2.y+p3.y+y+p4.y)/3 );
    n2.normalize();
    export.add( p1, p2, p3, n1 );
    export.add( p3, p2, p4, n2 );
    }
    }
}
```

- 8. Our exportInner() method is similar to the exportOuter() method, but the normals point in the opposite directions compared to the ones used in the exportOuter() method and the radius is reduced by 0.8.

```
void exportInner( TriangleSoup export ) {
for ( int h = 1; h < 19; h++ ) {
for ( int a = 0; a<72; a++ ) {
int ab = (a+1) % 72;
PVector p1 = new PVector( vertx[h][a] * 0.8, 95-h*5.0, verty[h][a] * 0.8 );
PVector p2 = new PVector( vertx[h-1][a] * 0.8, 95 - (h-1)*5.0, verty[h-1][a] * 0.8 );
PVector p3 = new PVector( vertx[h][ab] * 0.8, 95-h*5.0, verty[h][ab] * 0.8 );
PVector p4 = new PVector( vertx[h-1][ab] * 0.8, 95 - (h-1)*5.0, verty[h-1][ab] * 0.8 );
PVector n1 = new PVector( - (p1.x+p2.x+p3.x)/3, 0, - (p1.y+p2.y+y+p3.y)/3 );
n1.normalize();
PVector n2 = new PVector( - (p2.x+p3.x+p4.x)/3, 0, - (p2.y+p3.y+y+p4.y)/3 );
n2.normalize();
export.add( p1, p2, p3, n1 );
export.add( p3, p2, p4, n2 );
}
}
}
```

- 9. The exportBottom() method creates two triangle fans to close the bottom of our mesh. We need one bottom mesh for the outer wall and one that's slightly smaller and above it for the inner wall.

```
void exportBottom(TriangleSoup export) {
PVector n = new PVector( 0, -1, 0 );
PVector p1 = new PVector( 0, 5, 0 );
for ( int a = 0; a < 72; a++ ) {
int ab = (a+1) % 72;
PVector p2 = new PVector( vertx[18][a]*0.8, 5, verty[18][a]*0.8 );
PVector p3 = new PVector( vertx[18][ab]*0.8, 5, verty[18][ab]*0.8 );
export.add( p1, p2, p3, n );
}
n = new PVector( 0, 1, 0 );
p1 = new PVector( 0, 0, 0 );
for ( int a = 0; a < 72; a++ ) {
int ab = (a+1) % 72;
PVector p2 = new PVector( vertx[19][a], 0, verty[19][a] );
PVector p3 = new PVector( vertx[19][ab], 0, verty[19][ab] );
export.add( p1, p2, p3, n );
}
}
```

- 10. Finally, we need an exportTop() method that creates the top ring that connects the inner and the outer mesh.

```
void exportTop(TriangleSoup export) {
PVector n = new PVector( 0, 1, 0 );
for ( int a = 0; a < 72; a++ ) {
int ab = (a+1) % 72;
PVector p1 = new PVector( vertx[0][a], 95, verty[0][a] );
PVector p2 = new PVector( vertx[0][a]*0.8, 95, verty[0][a]*0.8 );
PVector p3 = new PVector( vertx[0][ab], 95, verty[0][ab] );
PVector p4 = new PVector( vertx[0][ab]*0.8, 95, verty[0][ab]*0.8 );
export.add( p1, p2, p3, n );
export.add( p3, p2, p4, n );
}
}
```

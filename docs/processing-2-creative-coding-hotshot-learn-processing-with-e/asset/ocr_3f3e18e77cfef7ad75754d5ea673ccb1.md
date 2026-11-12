To make our object more interesting, we added four additional control parameters to our GUI and extended the getR() function to take the height of the cylinder into account for calculating the radius.

# Exporting the object

We have created a 3D object in our previous task that can be changed using some sliders, but to print the object on a 3D printer, we have to save the object in a format that a 3D printer understands. Our current task is to add an export function that saves the object to an STL (STereo Lithography) file. This file format was originally invented to serve as an input format for Stereo Lithograph machines (hence the name), but nearly every currently available 3D software can import these files. The file stores the coordinates of the triangles and a normal vector for each of these triangles. This is why the format has the nickname Triangle Soup.

An STL file has an 80-byte header that is ignored by every known 3D program, so we set it to 0. Then, we need to write the count of the triangles as a four-byte unsigned long. This is followed by 12 float values for each triangle, which contain the normal vector and the coordinates of the vertices. Each triangle ends with a two-byte zero.

# Engage Thrusters

Let's export some triangles:

+   1. First, we add a class where we can store the triangles that our mesh is made of.
+   Add a new tab by clicking on the icon on the right and select the New Tab ... menu.
+   Name it Triangle and add the following code:

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
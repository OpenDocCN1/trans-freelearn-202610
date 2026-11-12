9. The exportBottom() method creates two triangle fans to close the bottom of our mesh. We need one bottom mesh for the outer wall and one that's slightly smaller and above it for the inner wall.

```
void exportBottom(TriangleSoup export) {
PVector n = new PVector( 0, -1, 0 );
PVector p1 = new PVector( 0, 5, 0 );
for ( int a = 0; a < 72; a++ ) {
int ab = (a+1) % 72;
PVector p2 = new PVector( vertx[18] [a]*0.8, 5, verty[18]
[a]*0.8 );
PVector p3 = new PVector( vertx[18] [ab]*0.8, 5, verty[18]
[ab]*0.8 );
export.add( p1, p2, p3, n );
}
n = new PVector( 0, 1, 0 );
p1 = new PVector( 0, 0, 0 );
for ( int a = 0; a < 72; a++ ) {
int ab = (a+1) % 72;
PVector p2 = new PVector( vertx[19] [a], 0, verty[19] [a] );
PVector p3 = new PVector( vertx[19] [ab], 0, verty[19] [ab] );
export.add( p1, p2, p3, n );
}
}
```

10. Finally, we need an exportTop() method that creates the top ring that connects the inner and the outer mesh.

```
void exportTop(TriangleSoup export) {
PVector n = new PVector( 0, 1, 0 );
for ( int a = 0; a < 72; a++ ) {
int ab = (a+1) % 72;
PVector p1 = new PVector( vertx[0] [a], 95, verty[0] [a] );
PVector p2 = new PVector( vertx[0] [a]*0.8, 95, verty[0] [a]*0.8
);
PVector p3 = new PVector( vertx[0] [ab], 95, verty[0] [ab] );
PVector p4 = new PVector( vertx[0] [ab]*0.8, 95, verty[0]
[ab]*0.8 );
export.add( p1, p2, p3, n );
export.add( p3, p2, p4, n );
}
}
```
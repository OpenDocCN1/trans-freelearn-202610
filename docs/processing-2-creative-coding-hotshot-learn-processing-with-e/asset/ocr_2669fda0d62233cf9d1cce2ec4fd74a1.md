```
ellipse( 120, 140, 60, 60);
ellipse( 180, 140, 60, 60);
fill(0);
ellipse( 125, 150, 20, 20);
ellipse( 175, 150, 20, 20);

line( 100, 100 + map( eye, 0, 1024, -10, 10 ), 140,
100 - map( eye, 0, 1024, -10, 10 ) );
line( 160, 100 - map( eye, 0, 1024, -10, 10 ),
200, 100 + map( eye, 0, 1024, -10, 10 ) );

noFill();
beginShape();
curveVertex(100, 200);
curveVertex(100, 200);
curveVertex(150, 180 + map( mouth, 0, 1024, 0, 50));
curveVertex(200, 200);
curveVertex(200, 200);
endShape();
}
```

10. Now run the sketch and change the values of our control variables to see how they affect the resulting face. The parameters we used in the example so far result in the smiley that you see in the following screenshot:

![](bbox=[0.385, 0.525, 0.625, 0.715])
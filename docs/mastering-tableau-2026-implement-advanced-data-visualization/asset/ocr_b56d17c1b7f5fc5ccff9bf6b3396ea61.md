+   5. Open a new workbook and name it 4-layer map. Create a calculated field called Line, with the following code:

```
MAKELINE(MAKEPOINT([Lat (Origin)], [Long (Origin)]), MAKEPOINT([Lat (Destination)], [Long (Destination)]))
```

+   6. The Tableau description shows that to make a point based on a spatial location; Tableau needs the longitude and latitude coordinates.

![](bbox=[0.09, 0.23, 0.86, 0.51])

Figure 9.31: MAKEPOINT

+   7. Create another calculated field called Days to delivery, with the following code:

```
[Delivered]-[Ordered]
```

+   8. Double-click on City from the Origin file and a map will appear, but you will encounter 12 unknowns. Click on the error and then Edit Locations...
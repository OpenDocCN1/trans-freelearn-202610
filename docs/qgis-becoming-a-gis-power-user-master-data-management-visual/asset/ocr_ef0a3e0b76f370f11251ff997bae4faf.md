+   2. Bring up the Raster calculator dialog.
+     1. Navigate to Raster | Raster calculator

+   3. Enter the map algebra expression.

+   - Add the raster layers by double-clicking on them in the Raster bands selection area
+   - Add the operators by typing them out or clicking on the buttons in the operators area
+   - The expression entered should be as follows:

```
("slope@1" < 8) + ("applicants@1" = 1) +
  ("easement_prox@1"<2000) + ("roads_prox@1">100) +
  ("developed_prox@1" > 500) + ("agriculture@1" < 100)
```

> [ 💡 @1 refers to the first and only band of the raster. ]

![](bbox=[0.215, 0.425, 0.785, 0.82])
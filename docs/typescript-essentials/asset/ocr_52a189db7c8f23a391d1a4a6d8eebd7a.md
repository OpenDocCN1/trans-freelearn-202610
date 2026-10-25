![](bbox=[0.06, 0.04, 0.48, 0.33])

Now we can start breaking our code into separate files, each serving a different and specific purpose. All of our interface types for a specific area can be placed into a single TypeScript file called <Area>Types.ts, for example, ShapeTypes.ts. This will give us a place to reference interfaces and enumerations while keeping our classes and actual implementation code separate. Each interface should be decorated with the export keyword to ensure that we are able to interact with all of these types in a consistent manner. Next, we need to decide how to structure our implementation code. Each of the different shape objects could be broken into a separate code file and placed inside the Shapes directory that we created. However, the amount of code in these classes is minimal and relatively easy to manage, so we can easily place them all inside of a single module. If the implementations start to grow in size like with the drawing-related objects, we will want separate files for each type to keep the code clear and easy to maintain.

The complete contents of the Shapes.ts file is shown in the following code:

```
import ShapeTypes = require('Scripts/Shapes/ShapeTypes');
export class Point implements ShapeTypes.IPoint {
    constructor(public X: number, public Y: number) {
    }
}
export class Line implements ShapeTypes.ILine {
    constructor(public p1: ShapeTypes.IPoint, public p2: ShapeTypes.IPoint)
{
    }
    public Length(): number {
        var a2 = Math.pow(this.p2.X - this.p1.X, 2);
        var b2 = Math.pow(this.p2.Y - this.p1.Y, 2);
        return Math.sqrt(a2 + b2);
    }
}
export class Rectangle implements ShapeTypes.IRectangle {
```
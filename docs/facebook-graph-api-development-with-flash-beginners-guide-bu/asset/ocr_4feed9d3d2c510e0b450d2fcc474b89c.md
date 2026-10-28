```
{
    public var rendererObject:GraphObjectRenderer;
    public var graphObjectListRenderers:Array = [];

    public function GraphObject()
    {

    }

}
}
```

Honestly, there's no need to worry about any of the code there. All you need to know is that it's dynamic, which means that we can assign new properties to it during runtime, without having to specify their names beforehand. So we can do this:

```
var graphObject:GraphObject = new GraphObject();
graphObject.favoriteColor = "red";
```

When a GraphObject is passed to the CustomGraphContainerController.renderGraphObject() method, every single property of the GraphObject will be rendered in a fancy list, automatically. Every single property apart from the two that are defined in the class already, that is!

So what we have to do, inside CustomGraphContainerController.onGraphDataLoadComplete(), is:

+   1. Create a new instance of GraphObject.
+   2. Copy all the properties of decodedJSON to this new GraphObject.
+   3. Pass the GraphObject to renderGraphObject().
+   4. The code for doing that is as follows:

```
private function onGraphDataLoadComplete(a_event:Event):void
{
    var loader:URLLoader = a_event.target as URLLoader;
    //obtain whatever data was loaded, and trace it
    var graphData:String = loader.data;
    var decodedJSON:Object = JSON.decode(graphData);

    var graphObject:GraphObject = new GraphObject();
    //copy all the properties from decodedJSON to graphObject
    for (var key:String in decodedJSON)
```
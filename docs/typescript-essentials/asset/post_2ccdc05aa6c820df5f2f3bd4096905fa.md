
```
this._drawingColor = (<any>e.currentTarget).value;
}, true);
}
public getNewShape(location: IPoint): IDrawingShape {
    var shape: IDrawingShape = null;
    var cursor: string = "auto";
    switch (this._drawingTool) {
        case DrawingToolType.Rectangle:
            shape = new DrawingRectangle();
            shape.Move(location);
            (<DrawingRectangle>shape).shape.height = 3;
            (<DrawingRectangle>shape).shape.width = 3;
            (<DrawingRectangle>shape).fillStyle = this._drawingColor;
            cursor = "se-resize";
            break;
        case DrawingToolType.Line:
            shape = new DrawingLine();
            (<DrawingLine>shape).shape.p1 = location;
            (<DrawingLine>shape).shape.p2 = new Point(location.x + 1, location.y + 1);
            (<DrawingLine>shape).strokeStyle = this._drawingColor;
            cursor = "e-resize";
            break;
    }
    window.document.body.style.cursor = cursor;
    return shape;
}
```

As you can see, we added a new private variable to store the current color value. The extra event handler was added to listen for the color picker’s value to change and to store this value. The getNewShape code simply sets the appropriate property for the type being instantiated and the shape object handles the rest. As you can see in the following screenshot, we can now create objects of any size or color:

## Drawing Application

Select   Rectangle   Line

![](img/83a7f7e1723158e56008cf70304a8da4_176_0.png)

![](img/83a7f7e1723158e56008cf70304a8da4_176_1.png)

## Color

Basic colors:

Custom colors:

Define Custom Colors >>

OK   Cancel

Color|Solid

Hue: 90
Sat: 240
Lum: 120
Red: 0
Green: 255
Blue: 64

Add to Custom Colors

## Summary

Throughout this chapter, we have focused on building a drawing application from end to end in TypeScript. We started with the basic building blocks for the application, the abstraction, then created a set of reusable shape objects. These objects are then used in a set of classes responsible for drawing them on an HTML5 canvas. Next, we built an engine responsible for handling user interaction and requesting information from the drawing objects to make decisions. Finally, we built a model to keep track of all of the shape objects in the running application. All of this has been done using TypeScript, and only TypeScript. However, a common practice in web development is integrating with third-party libraries such as jQuery. In the next chapter, we are going to look at some of the available third-party libraries and how to integrate them with your projects.

## Chapter 6. Declaration Files and Library Integrations

So far, we have been writing TypeScript without integrating with any external JavaScript libraries. We have covered the different concepts that the language adds on top of JavaScript and how to use them to our advantage when building large scale applications. We have ignored all of the third-party libraries that are openly available on the Web to improve the JavaScript experience. In this chapter, we will cover declaration files and how they help us integrate with other JavaScript libraries. The topics we are going to cover in this chapter include:

- Declaration files
- The NuGet package manager
- jQuery
- Knockout
- External modules

## Declaration files

Declaration files are a special type of source file in TypeScript and have a different file extension. Declaration files have a file extension of `.d.ts` and they can contain type information but no implementation details. This includes interface definitions as well as type declarations.

Type declarations are created using the `declare` keyword, as shown in the following screenshot:

![](img/83a7f7e1723158e56008cf70304a8da4_181_0.png)

As you can see, the name of the file is `Book.d.ts`, which is how the compiler knows that this file will only contain declarations. When this occurs, no output file is created and an error will be generated when the implementation code is found in the file. The purpose of these files is to provide type information for other JavaScript libraries that are not in TypeScript files. This allows us to interact with these libraries in a strongly typed fashion, providing compile time checks and intelligent code completion. In the previous example, we declare that the Book class will exist in the global namespace allowing it to be referenced throughout our project. We can now use Book freely, but if an implementation is not provided for it our application will fail during execution wherever the Book type is referenced.

As we saw earlier on when looking at the TypeScript compiler and its options, we can automatically generate declaration files for our own code. These declaration files can be referenced from other projects within your own solution or deployed as part of a library you distribute. A declaration file will be output for every JavaScript file that is output. If you combine all of your code into a single file then a single declaration file will be created. Selecting the **Generate declaration files** option from the TypeScript build section of the project properties will provide the compiler with the appropriate parameter used to create our declaration files, as shown in the following screenshot:

![](img/83a7f7e1723158e56008cf70304a8da4_181_1.png)

Throughout this book, we have used a variety of types that weren’t explicitly declared by us. The CanvasRenderingContext2D object we used in our drawing application had a full list of type information in Visual Studio IntelliSense, yet we never saw its declaration or implementation. This is because TypeScript comes with a lib.d.ts file that declares type information for thousands of different objects. This declaration file is automatically referenced every time the compiler runs. This list of types is incredibly useful but it doesn’t include any types from external libraries that may or may not be referenced in your projects.

## Third-party library integration

One of the key components in large scale web development is the use of open source libraries that handle certain tasks for us. jQuery is one of the more popular libraries and is used for easy interaction with the **Document Object Model (DOM)**. These libraries provide a wide variety of functionalities but all of these interactions can be given some level of typing. In this section, we are going to look at the following libraries:

- jQuery
- Knockout
- RequireJS

Before we start developing these libraries, we need to get them referenced in our project. Each of these libraries can be downloaded directly from their web pages or through the use of NuGet. NuGet is a package manager for Visual Studio and can be installed as an extension.

![](img/83a7f7e1723158e56008cf70304a8da4_186_0.png)

The NuGet packages we will be installing will include the code for the libraries we will be using as well as separate declaration files for each of these libraries. Not every library has NuGet packages or declaration files associated with them, however, the web development community is very good about filling in these holes. There is a large repository of TypeScript library declaration files available from the **DefinitelyTyped** repository on GitHub.

## Integrating with jQuery

jQuery is one of the most popular open source libraries available on the Web. It can be used for anything from DOM manipulation to simplifying remote server calls. If you plan on building a large scale web application you were probably already planning on using jQuery and if not, I strongly recommend that you do. To start using jQuery, the first thing we need to do is install the jQuery NuGet package. Right-click on the project and select **Manage NuGet Packages**. This will bring up a window similar to the updates and extensions window. Locate jQuery from the nuget.org repository in the **Online** section and install it. As you can see in the following screenshot, several files are installed with this package that help us develop with jQuery:

![](img/83a7f7e1723158e56008cf70304a8da4_187_0.png)

Now we can add a script tag the index.html file and jQuery will be available at runtime for us to use. This, however, isn’t very useful during development and compilation of our TypeScript code because we don’t have any type information about the library. To import this type information, we need a declaration file that represents the jQuery library. Open up the **Manage NuGet Packages** window again and search for jQuery.TypeScript. Install the package named **jquery.TypeScript.DefinitelyTyped**, which is on Version 1.3.5 at the time of writing this, to import the type information of the jQuery library.

### Tip

There are a number of other libraries that extend jQuery and quite a few declaration files have been created to support these different extensions.

Now that the type information for jQuery has been included in the project we will be able to use the library in a more robust way. Let’s add some elements to our HTML that we will manipulate with the jQuery library.

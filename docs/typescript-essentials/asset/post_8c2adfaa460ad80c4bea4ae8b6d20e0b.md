
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8" />
    <title>TypeScript App</title>
    <link rel="stylesheet" href="app.css" type="text/css" />
    <link rel="stylesheet" href="DrawingApplication.css" type="text/css" />
    <script src="Scripts/jquery-2.1.1.js"></script>
    <script src="Scripts/knockout-3.1.0.js"></script>
    <script data-main="app" src="Scripts/require.js"></script>
</head>
<body>
    <h1>Drawing Application</h1>
    <div id="content">
        <div data-bind="foreach: userControls">
            <span data-bind="attr: { id: id }, template: { name: templateName }"></span>
        </div>
        <canvas id="drawingCanvas" height="400" width="570" />
    </div>
</body>
</html>
```

When we run the application, the **Select** button will display the selected style because the KnockoutComputed object we added evaluated to true for that control. Selecting any of the other tools will cause the bindings to be re-evaluated and the styles to change with the user action. Any of the elements with a border on them have a consistent color, and if we ever want to modify that, we will only have to change it in a single location. This will save us a significant amount of time if we ever need to restyle the application.

## Drawing Application

Select Rectangle Line

## Summary

In this chapter, we have covered several ways to improve the way we develop and deploy enterprise-level JavaScript applications. We converted the drawing application to AMD modules to eliminate the need for manually managing how scripts are loaded. We created bindings between our HTML and TypeScript code using Knockout, which allow us to easily extend the behavior of our application with minimal effort. Then, we looked to improve the speed at which our application is available by reducing all of our modules into a single optimized JavaScript file. Finally, we added LESS style sheets that allowed us to create a robust set of styles that could be easily maintained and modified. In the next chapter, we will look at the different methods used to debug TypeScript as well as how we can provide unit tests for our application.

## Chapter 8. Debugging TypeScript

We have covered all the skills you will need to write large-scale applications using TypeScript. The compiler and language constructs provided by TypeScript allow us to quickly and safely develop reliable programs. As all experienced developers know though, code that compiles doesn’t always perform as expected. In this chapter, we will discuss the different options available to debug TypeScript once it is deployed and running. We will also cover unit testing, which will allow us to test the functionality with code. In this chapter, we will investigate the following topics:

- Debugging
- Test-driven development

### Debugging

When developing plain old JavaScript applications, our debugging options are usually dependent upon the available tooling in each browser. Most modern browsers include some form of developer console that allows us to view the scripts running on a web page. From this console, we can set breakpoints and investigate the behavior of our application. We can perform the same task in TypeScript applications; however, this leaves us stepping through the generated JavaScript rather than the TypeScript code we actually wrote.

### Source maps

As we discussed earlier, source maps are a way to map one code file, generally minified, back to the original code file. In TypeScript, this concept holds true and allows us to step through our non-compiled TypeScript code. To enable this functionality, the first thing we must do is ensure that our project settings are set to generate source maps; this setting is on by default:

![](img/83a7f7e1723158e56008cf70304a8da4_237_0.png)

This setting will provide the --sourcemap parameter to the compiler and allow us to link the output JavaScript file to the original TypeScript file. In most modern browsers, this will cause the TypeScript file to download to the client and we will be able to set breakpoints and step through our code line by line.

### Debuggers

There are several ways to debug our application’s code, and it is likely that we will need a combination of them to ensure we provide a high quality application. Modern browsers such as Chrome, Firefox, and Internet Explorer all include a developer console that provides debugging features for web applications. In the following screenshot, you can see that we can step directly into our TypeScript code using the Chrome developer tools:

![](img/83a7f7e1723158e56008cf70304a8da4_238_0.png)

Here, you can see that we have set a breakpoint on the `Draw` method of the `CanvasEngine` of our drawing application. In the source window, both the JavaScript file and the TypeScript file are visible, as are the type annotations we included in the TypeScript file. This is an incredibly powerful feature for developers because it prevents us from having to translate the generated JavaScript code back to a particular line of the original TypeScript code. With Visual Studio and Internet Explorer, we are actually able to debug directly from the IDE. This enables us to use all of the tools and features available in Visual Studio in the same way that we could debug C# code. This includes the call stack, adding any watch expressions, as well as object exploration, as you can see in the following screenshot:

It is important to test our application’s code in all of the environments that it is expected to run in. Each browser implements its own JavaScript engine and has adopted different sets of features from the HTML5 standard. Being familiar with each of the different developer consoles will be important to track down and resolve issues that may not be readily apparent in another browser.

### Test-driven development

As our applications grow in both size and complexity, it is more important than ever for us to deliver software with as few bugs as possible. Manual testing is inconsistent and slow, which means we need to adopt a more methodical way of ensuring the quality of our software. This need has brought about the notion of test-driven development. Test-driven development is a development cycle that repeats a small set of steps to ensure each line of code is tested. The steps in the test-driven development life cycle are:

- Adding a test
- Running the tests
- Writing the code
- Running the tests again
- Refactoring the code

The first thing we want to do is write a test to verify some functionality. This creates a requirement for the code that we have yet to write. This test will initially fail when writing unit tests for new functionality; this is expected and helps to verify that the test does have a failure state. At this point, we run all of the tests, verifying that preexisting tests still pass and our new test fails. If the new test does not fail, the functionality has either already been implemented or the test is invalid and needs to be rewritten. Once we have a valid test, we must implement the feature that we are attempting to test. This code does not have to be perfect upon the first implementation; the goal is only to write enough code to pass the test. When the functionality is implemented, we rerun the tests and verify that our code is effective. If not, then it must be corrected. Otherwise, the code can be refactored; but the test must always pass. This process is repeated until every feature has tests and an implementation. Now that we are familiar with the process of test-driven development, let’s look at how unit testing works in TypeScript.

# Note

For more information about test-driven development, please visit http://msdn.microsoft.com/en-us/library/aa730844(v=vs.80).aspx.

### Unit testing

Unit tests are code blocks that test small units of functionality with a set of control data and expected results. These tests are reliable and reusable, ensuring that our code is not only tested for the current version of the application, but for all future releases. This will ensure that any added or modified features do not break existing functionality and will alert us of a problem well ahead of time. TypeScript does not come with any built-in unit testing tools; however, the web community once again filled this hole. A unit test framework has been created by Maxim Fridberg and gives you a simple way to create and view all of your unit tests for libraries that you have created. The name of the framework is MaxUnit and is available for download from https://github.com/KnowledgeLakegithub/MaxUnit, or it is available as a NuGet package called KL.Testing.MaxUnit.

![](img/83a7f7e1723158e56008cf70304a8da4_242_0.png)

This package will add a number of files to our Visual Studio project including sample usage of the framework. The advantage of MaxUnit is that it allows us to write tests in TypeScript for our TypeScript libraries. There are a number of other testing frameworks available to test JavaScript libraries, including Jasmine (https://github.com/pivotal/jasmine/) and QUnit (http://qunitjs.com/). It is generally best to keep your test code separate from the implementation code, to ensure that it will be tested in the same manner that it will be used. To get a feel for how unit testing works, let’s take the Shapes library that we created for the drawing application and create a series of tests to ensure proper functionality. The Shapes library outputs to a single JavaScript file and has interfaces associated with it that we can use to test our code.

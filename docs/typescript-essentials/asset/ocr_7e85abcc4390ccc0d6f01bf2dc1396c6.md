# JavaScript output

Up until this point, we have only been working with compiling single TypeScript files at a time. As applications grow in size though, it is very important to separate our code into maintainable segments. When writing code for a C# application, it is common to separate code files by class. JavaScript conventions vary in how code is segmented. If following the module pattern, it is common to place an entire module in a single file; otherwise, code is usually segmented into related blocks as the application requires them. Passing multiple file names separated by spaces will tell the compiler to parse each file and generate a JavaScript file for each. The following screenshot shows how this is done:

![](bbox=[0.06, 0.25, 0.69, 0.37])

As you can see, we now have two TypeScript files and each is being passed to the compiler as a parameter. The result of running this command will be two JavaScript files, one for each corresponding TypeScript file. There is no limit to the number of files that can be provided; however, at some point the command line will reach its buffer limit. We will see in a little while how to make this easier through the use of a parameter file.

The TypeScript compiler provides two main options for controlling where the JavaScript that will be used at runtime is output to. The first option requires a folder location to output all of the generated JavaScript files separately. As you can see in the following screenshot, we use the --outDir parameter to provide our location:

![](bbox=[0.06, 0.62, 0.69, 0.77])

The other option allows us to compile our TypeScript down into a single JavaScript file. This allows us to make a single HTTP GET request for our application rather than having to make multiple ones. This can be a very complex task to accomplish when an application grows in scale and each piece of code must be loaded in a certain order to ensure proper execution. The TypeScript compiler has a few different ways of ensuring that code generated in a single file is created in such a manner that all objects exist before
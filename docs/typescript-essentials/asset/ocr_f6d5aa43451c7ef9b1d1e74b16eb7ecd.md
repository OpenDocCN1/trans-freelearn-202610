# Build options

Visual Studio has provided a seamless integration with TypeScript and provides a large variety of configuration options. Let’s take a look at the project build options first and get a feel for the different things that TypeScript can generate when it is being compiled. To find the build options, right-click on the project in the solution explorer and select Properties from the context menu. As you can see in the following screenshot, we are able to manipulate a large number of the compiler options we saw earlier:

![](bbox=[0.06, 0.22, 0.84, 0.71])

From the build settings you can see that we have the ability to change which version of JavaScript we want to compile our TypeScript into: ECMAScript 5 or ECMAScript 3.

# Tip

JavaScript is now formally known as ECMAScript but it is still commonly referred to as its original name.

You also have the option for compiling TypeScript files when you save changes to them. This is a helpful option because when a TypeScript file is compiled so are all other TypeScript files that it references. This is a helpful feature when you have only a few files

Notice that we're specifying version numbers here – these are the versions that I used while writing the code for this book. You can try later versions if you want, as long as they are minor revisions. Major revisions may work, but you may also have to make modifications to the code in this book in order to support them.

### Tip

It's a good idea to be positioned in your project's root directory any time you execute an `npm` or `gulp` command. On Linux and OS X, these commands generally locate the project's root directory automatically; but this isn't guaranteed on all platforms, so it's better to be safe than sorry.

That's it! Gulp itself is very easy to install, but most workflows require additional plugins that work with Gulp. In addition, we'll also install the Cordova dependencies for this project.

### Note

If you're working with the code bundle for this chapter, you can install all the following dependencies by executing `npm install`.

First, let's install the Cordova dependencies:

```
$ npm install --save-dev cordova-lib@5.4.1 cordova-ios@3.9.2 cordova-android@4.1.1
```

`cordova-lib` allows us to programmatically interact with Cordova. We can create projects, build them, and emulate them—everything we do with the Cordova command line can be done with `cordova-lib`. `cordova-ios` and `cordova-android` refer to the iOS and Android platforms that `cordova platform add ios android` would add. We've made them dependencies for our project, so we can easily control which version we will build it with.

### Tip

While starting a new project, it's wise to start with the most recent version of Cordova and the requisite platforms. Once you begin, it's usually a good practice to stick with a specific platform version, unless there are serious bugs or security issues that require updating to a newer platform version.

Next, let's install the Gulp plugins we'll need:

```
$ npm install --save-dev babel-eslint@4.1.1 cordova-tasks@0.2.0 gulp-babel@5.2.1 gulp-bump@0.3.1 gulp-concat@2.6.0 gulp-eslint@1.0.0 gulp-jscs@3.0.2 gulp-notify@2.2.0 gulp-rename@1.2.0 gulp-replace-task@0.10.1 gulp-sourcemaps@1.3.0 gulp-uglify@1.4.0 gulp-util@3.0.6 merge-stream@1.0.0 require-all@1.1.0 rimraf@2.4.3
```

These will take a few moments to install; but when you're done, take a look at `package.json`. Notice that all the dependencies we added are also added to the `devDependencies` section in the file. This makes it easy to install all the project's dependencies at a later date (say, on a new machine) simply by executing `npm install`.

Before we go on, let's quickly go over what each of the earlier mentioned utilities do. We'll go over them in more detail as we progress through the remainder of this chapter:

*   `gulp-babel`: Converts ES2015 JavaScript into ES5. If you aren't familiar with ES2015, it has several new features and an improved syntax that make writing mobile apps easier. Unfortunately, because most browsers don't yet natively support the ES2015 features and syntax, it must be transpiled to the ES5 syntax. Of course, if you prefer other languages that can be compiled to ES5 JavaScript, you could use those as well (these would include CoffeeScript and so on).
*   `gulp-bump`: This small utility manages the version numbers in `package.json`.
*   `gulp-concat`: This concatenates streams together. We can use this to bundle files together.
*   `gulp-jscs`: This performs the JavaScript code style checks against your code. It supports ES2015.
*   `gulp-eslint`: This lints your JavaScript code. It supports ES2015.
*   `babel-eslint`: This provides ES2015 support to `gulp-eslint`.
*   `gulp-notify`: This is an optional plugin, but it comes in handy, especially when some of your tasks take a few seconds to run. This plugin will send a notification to your computer's notification panel when something of import occurs—perhaps an error or a completion message. If the plugin can't send it to your notification panel, it will be logged to the console.
*   `gulp-rename`: This renames the streams.
*   `gulp-replace-task`: This performs text searches and replaces within the streams.
*   `gulp-sourcemaps`: While transpiling ES2015 to ES5, it can be helpful to have a mapping between the original source and the transpiled source. This plugin creates them automatically for you.
*   `gulp-uglify`: This uglifies/minifies your code. While useful for code obfuscation, it also reduces the size of the code.
*   `gulp-util`: This provides additional utilities for Gulp, such as logging.
*   `merge-stream`: This merges multiple tasks.
*   `require-all`: This lets us import an entire directory of code into an object at once.
*   `rimraf`: Easy file deletion. Akin to `rm` on the command line.

Gulp tasks are defined by the contents of the project's `gulpfile.js` file. This is a JavaScript program, so the same skills you have with JavaScript will apply here. Furthermore, it's executed by Node.js, so if you have any Node.js knowledge, you can use it to your advantage.

### Tip

This file should be placed in the root directory of your project and must be named `gulpfile.js`.

The first few lines of your Gulp configuration file will `require` the Gulp plugins you'll need to complete the tasks. The following lines will then specify how the various tasks need to be performed. For example, a very simple configuration might look as follows:

```
var gulp = require("gulp");
gulp.task("copy-files", function () {
  gulp.src(["./src/**/*"])
      .pipe(gulp.dest("./build"));
});
```

This configuration only performs one task: it moves all the files contained within `src/` to `build/`. In many ways, this is the simplest form of a build workflow, but it's a bit too simple for our purposes.

### Tip

Note the pattern we used to match all the files. If you need to see the documentation on what patterns are supported, see [https://www.npmjs.com/package/glob](https://www.npmjs.com/package/glob).

To execute the task, you can execute `gulp copy-files`. Gulp would then execute the task and copy all the files from `src/` to `build/`.

What makes Gulp so powerful is the concept of task composition. Tasks can depend on any number of other tasks and those tasks can depend on yet more tasks. This makes it easy to create complex workflows out of simpler pieces. Furthermore, each task is asynchronous, so it is possible for many tasks with no shared dependencies to operate in parallel.

Each task, as you can see in the prior code, is comprised of a selection of a series of source files (`src()`), optionally performing some additional processing on each file (via `pipe()`) and then writing those files to a destination path (`dest()`). If no additional processing is specified (as in the prior example), Gulp will simply copy the files that match the wildcard pattern. The beauty of streams, however, is that one can execute any number of transformations before the final data is saved. So, the workflows can become very complex.

Now that you've seen a simple task, let's get into some more complicated tasks in the next section.

Although you can add all the tasks that you want to run to a single configuration file, this quickly becomes unwieldy as you add more tasks to your environment. In order to keep your configuration maintenance easy, it's best to split everything up into separate files.

This means that aside from our project's directory structure, our Gulp configuration has its own structure. The following shows how I like to structure my configuration:

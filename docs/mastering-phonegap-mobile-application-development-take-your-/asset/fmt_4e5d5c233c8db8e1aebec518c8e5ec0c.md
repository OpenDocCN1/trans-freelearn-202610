
# Browserify 和 ES2015 集成

```javascript
function copyCode() {
  var isRelease = (settings.BUILD_MODE === "release");
  return browserify(paths.makeFullPath(config.assets.code.src,
                    paths.SRC), { // [1]
    debug: !isRelease, // [2]
    standalone: "app"
  }).transform(babelify.configure({ // [3]
      stage: 0, // allow experimental features // [4]
    }))
    .bundle()
    .on("error", function(error) {
      this.emit("end"); // [5]
    }).on("error", notify.onError(function(error) {
      return "BABEL: " + error.message; // [6]
    }))
    .pipe(source("app.js")) // [7]
    .pipe(buffer()) // [8]
    .pipe(cordovaTasks.performSubstitutions())
    .pipe(isRelease ? gutil.noop() : sourcemaps.init({
      loadMaps: true
    })) // loads map from browserify file
    .pipe(isRelease ? uglify({
      preserveComments: "some"
    }) : gutil.noop())
    .pipe(isRelease ? gutil.noop() : sourcemaps.write())
    .pipe(gulp.dest(paths.makeFullPath(config.assets.code.dest, 
          paths.DEST)))
    .pipe(browserSync.stream());
}
```

前面代码的后半部分看起来应该很熟悉——这就是上一章的内容，但上半部分是全新的。让我们更详细地了解一下。

首先，我们没有使用 `gulp.src`，而是直接调用了 `browserify`（`[1]`）。在上一章中，我们让 Gulp 将所有 JavaScript 文件合并成一个文件。而 `browserify` 会通过从主文件开始跟踪依赖树，自动发现所有需要打包在一起的文件。那些不是依赖的文件会自动被排除在最终输出之外。仅这一特性就非常不错。

在调用 `browserify` 时，我们通过设置 `debug` 属性（`[2]`）让它创建 sourcemap。我们只在开发时需要这样做，所以这取决于我们传递给 Gulp 的 `--mode` 设置。

我们还希望在测试时能全局访问应用程序的状态。默认情况下，`browserify` 会将所有内容打包成一个整齐的包，但我们无法直接访问应用程序可能创建的对象。向 `browserify` 传递 `standalone:"app"` 允许我们指定一个全局变量（`app`），通过它可以访问整个应用。这对生产代码不是必需的；但在开发调试时绝对有帮助。

接下来，我们使用 `babelify`（`[3]`）转换代码。`browserify` 会自动为我们调用 `babel`。我们还传递了 `stage: 0`（`[4]`），以便使用 `await` 和 `async` 等 ES2016 特性。

还添加了几个错误处理器，以便在 `browserify` 转换代码时遇到问题能收到通知。第一个（`[5]`）结束流（如果你想让 Gulp 监视代码，这很重要），第二个（`[6]`）生成错误消息。

最后，我们进入了 Gulp 的领域——前两个 `pipe` 方法是我们将 Gulp 引入的地方（`[7],[8]`）。因为我们没有让 Gulp 查找特定文件，所以使用了之前安装的两个 `vinyl` 模块，将 `browserify` 的输出转换为 Gulp 可处理的形式。从这一点开始，我们的代码与[第 1 章](#ch01.html "Chapter 1. Task Automation")《任务自动化》中的代码完全相同。

## 引入 Node.js 包

引入 Node.js 包非常简单。首先，你需要从 `npm` 找到一个喜欢的包。在示例中，我们将添加一个名为 `yasmf-h` 的模块。它不是一个复杂的包，本质上是[第 1 章](#ch01.html "Chapter 1. Task Automation")《任务自动化》中 `index.js` 里那个小模板方法的增强版。

要安装该包，请输入以下命令：

```bash
$ npm --save install yasmf-h
```

接下来，我们需要修改 `src/www/js/app/index.js` 以便使用该包：

```javascript
import 'babel/polyfill'; // [1]
import h from 'yasmf-h'; // [2]

export var app = {
  start() {
    var n = h.el("div",
      h.el("ul",
        h.el("li", "Some information about this app..."),
        h.el("li", "App name: {{{NAME}}}"),
        h.el("li", "App version: {{{VERSION}}}")
      )
    );
    h.renderTo(n, document.querySelector("#demo"));
  }
};

document.addEventListener("deviceready", app.start, false);
```

这与[第 1 章](#ch01.html "Chapter 1. Task Automation")《任务自动化》中的内容没有本质区别。注意这两条 `import` 语句。第一行代码（`[1]`）导入支持 Babel 所需的 `polyfill`，Babel 是我们一直在使用的 ES2015 转译器。第二行（`[2]`）导入我们安装的 `npm` 模块的代码，并将其赋值给变量 `h`。

我们还需要稍微修改 `src/www/index.html` 文件，即删除一行。请看下面的内容：

```html
<!DOCTYPE html>
<html>
  <head>
    <script src="cordova.js" type="text/javascript"></script>
    <script src="./js/app/app.js" type="text/javascript"></script>
  </head>
  <body>
    <p>This is static content..., but below is dynamic content.</p>
    <div id="demo"></div>
  </body>
</html>
```

一切都一样，只是我们删除了以下行：

```html
<script src="./js/lib/browser-polyfill.js" type="text/javascript"></script>
```

我们不再需要在 `index.html` 中引入 ES2015 polyfill，因为已经在 `index.js` 中导入了它。`index.html` 中引入的脚本越少越好。

此时，你可以使用 `gulp build` 重新构建项目以应用新更改。它将完全像以前一样运行，但现在你正在使用一个*你没写的*第三方模块！

当然，在使用你没写的模块时，有几件事非常重要：

*   始终检查许可证。`npm` 包通常相当宽松，但最好复查一下。我们引入的包采用 MIT 许可证，要求仅在输出中某处包含原始版权。因为许可证包含在包的代码中，所以会被传递到我们的输出代码中。但确保正确授权项目中包含的任何代码取决于你。你还应该在应用的显眼部分（如 **About** 或 **Settings** 部分）列出你使用的包。

*   安装前审查包。查看代码或文档，看看是否有无法绕过的重大问题。如果包有测试，更好——你可以验证包是否真正工作。

本章涵盖了相当多的内容——大多与 ES2015 带来的新语法和特性相关。此外，我们还介绍了如何导入 `npm` 模块并在自己的代码中使用它们。

在下一章中，我们将致力于使用 Sass 构建模块化 CSS 文件。我们还将看到如何将其集成到构建过程中。

虽然 JavaScript 对应用程序功能至关重要，但 CSS 对应用程序外观至关重要。虽然 CSS 非常强大，但大多数移动浏览器支持的版本（CSS3）缺少许多能让开发更轻松的特性。CSS 也往往冗长；很容易最终得到又长又乱、难以理解和维护的样式表。

Sass（Syntactically Awesome Style Sheets）旨在改进 CSS3，使其更易于编写和维护。它增加了变量、插值、计算、混入、模块化等特性。我们可以利用 Sass 让 CSS 更易读、更易写。

### Note

你可以访问 `http://sass-lang.com` 了解更多关于 Sass 的信息。

在本章中，我们将重点关注以下内容：

*   学习 Sass
*   将 Sass 与 Gulp 集成

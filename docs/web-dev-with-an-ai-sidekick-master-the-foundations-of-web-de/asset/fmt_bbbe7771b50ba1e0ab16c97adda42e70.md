
# TypeScript 设置与基础类型

## 验证工具链

输出应为：

```
Hello, TypeScript
```

如果正常工作，说明你的工具链运行正常。

在浏览器中调用 `console.log` 时，它会写入 **开发者工具** 中的 `Console` 标签页。在 Node.js 运行的代码中调用 `console.log` 时，它会写入标准输出，如命令提示符或终端窗口。

## 引入 tsconfig.json

对于超出玩具示例范围的项目，你不想每次都向 `tsc` 传递选项。取而代之的是，在项目根目录创建一个名为 `tsconfig.json` 的配置文件。

在 `MyFirstTS` 文件夹中，生成一个初始配置：

```
tsc --init
```

这会创建一个包含许多注释选项的文件：

```
{
  // Visit https://aka.ms/tsconfig to read more about this file
  "compilerOptions": {
    // File Layout
    // "rootDir": "./src",
    // "outDir": "./dist",
    // Environment Settings
    // See also https://aka.ms/tsconfig/module
    "module": "nodenext",
    "target": "esnext",
    "types": [],
    // For nodejs:
    // "lib": ["esnext"],
    // "types": ["node"],
    // and npm install -D @types/node
    // Other Outputs
    "sourceMap": true,
    "declaration": true,
    "declarationMap": true,
    // Stricter Typechecking Options
    "noUncheckedIndexedAccess": true,
    "exactOptionalPropertyTypes": true,
    // Style Options
    // "noImplicitReturns": true,
    // "noImplicitOverride": true,
    // "noUnusedLocals": true,
    // "noUnusedParameters": true,
    // "noFallthroughCasesInSwitch": true,
    // "noPropertyAccessFromIndexSignature": true,
    // Recommended Options
    "strict": true,
    "jsx": "react-jsx",
    "verbatimModuleSyntax": true,
    "isolatedModules": true,
    "noUncheckedSideEffectImports": true,
    "moduleDetection": "force",
    "skipLibCheck": true,
  }
}
```

你目前不需要理解所有选项。只需掌握基础即可：

- `compilerOptions` 控制 TypeScript 的严格程度及其生成的 JavaScript 类型
- `target` 控制输出中允许使用哪些 JavaScript 特性
- `strict` 启用更安全的类型检查规则
- `jsx` 用于 React 项目，本书不涉及，可忽略或移除此选项

**提示**：TypeScript 严格模式在底层实际启用了哪些检查？即使启用了严格模式，TypeScript 仍无法捕获哪些类型的 Bug？

一个适合初学者的简单配置如下：

```
{
  "compilerOptions": {
    "target": "ES2020",
    "strict": true,
    "outDir": "dist"
  }
}
```

现在，无需编译单个文件，直接运行即可：

```
tsc
```

TypeScript 会编译项目中所有 `.ts` 文件，并将输出的 JavaScript 放入 `dist` 文件夹。

`target` 设置控制 TypeScript 编译代码时生成的 JavaScript 版本。这影响允许使用的语言特性以及语法转换方式。常见选项包括：

- `ES3`：早期 TypeScript 版本的历史默认值，当时仍需支持极旧的浏览器（如 Internet Explorer 8）。实际上，几乎没有当前项目应依赖此默认值。ES3 缺乏对许多标准 JavaScript 特性的支持，TypeScript 会激进地转换代码，往往产生冗长且难以阅读的输出。
- `ES5`：非常古老的 JavaScript，几乎所有浏览器均支持。仅适用于遗留环境。
- `ES2015` (`ES6`)：引入类、箭头函数和模块。适合作为较旧但仍广泛支持环境的基准。
- `ES2017` / `ES2018` / `ES2019`：渐进式改进，如 async/await (`ES2017`) 及其他现代特性。常用于面向稍旧版本的 Node.js 或浏览器。
- `ES2020`：流行的基准。支持可选链和空值合并等特性，仅需最少转换。
- `ES2022` 及更高版本 (`ESNext`)：非常新的 JavaScript，几乎无需降级转换。最适合当前 Node.js 版本或现代浏览器。

**最佳实践**：始终自行设置 `target` 为 `ES2020` 或更高版本。默认值仅为向后兼容而存在，并非当今的良好选择。

**提示**：请解释对初学者最重要的 `tsconfig.json` 选项，以及哪些选项初期可以安全忽略。

## 平台说明（Windows 与 macOS 与 Linux）

从 TypeScript 角度来看，平台差异极少。值得注意的点：

- TypeScript 编译器在所有平台行为一致
- `.ts` 文件可在所有操作系统间移植
- 输出的 `.js` 文件在浏览器中运行方式相同

主要差异在于：

- 安装 Node 或全局包的命令行语法
- Linux 和 macOS 上的文件系统大小写敏感性
- 路径分隔符 (`\` 与 `/`)，工具通常会自动处理

只要避免硬编码路径并依赖标准工具，你的设置将在各平台上同等工作。

## 设置总结

此时，你应理解设置 TypeScript 实质上意味着三件事：

1. 安装 Node.js
2. 安装 TypeScript 编译器
3. 创建带有配置文件的基础项目

其他一切——框架、打包工具、高级构建工具——均构建在此基础之上。完成此设置后，你已准备好开始学习类型与接口。

# 基础类型与接口

这是 TypeScript 开始证明其存在价值的地方。你已懂得如何编写 JavaScript。TypeScript 新增的是以精确、可检查的方式描述数据的能力。一旦理解一小组基础类型及接口的工作原理，语言的其余部分将变得容易得多。

## 基础类型

TypeScript 从你在 JavaScript 中已使用的相同原始值开始，但允许你显式声明它们。

变量可通过 **类型注解** 赋予类型。注解位于变量名后，以冒号分隔：

```
let count: number = 0;
let title: string = "Dashboard";
let isLoggedIn: boolean = false;
```

上述代码中 TypeScript 所做的：

- `count` 必须始终持有数字
- `title` 必须始终持有字符串
- `isLoggedIn` 必须始终持有布尔值

若稍后尝试赋值不兼容的值，TypeScript 会立即标记错误：

```
count = "zero";       // Error
isLoggedIn = "yes";   // Error
```

这些错误会在你输入时直接显示在编辑器中。代码可能仍能编译（取决于设置），但你能在运行时前尽早收到警告。

**提示**：TypeScript 如何决定即使存在类型错误代码仍能编译成功？

字符串值得特别一提。TypeScript 完全支持 JavaScript 模板字面量并正确推断其类型：

```
let userName: string = "Mark";
let message: string = `Hello, ${userName}`;
```

**提示**：`never` 类型是什么？它在实际代码中何种情况下出现？

## 类型推断

你不总需要编写类型注解。TypeScript 常能从初始值推断类型：

```
let total = 100;       // 推断为 number
let label = "Total";   // 推断为 string
```

从此刻起，`total` 行为如同你写了 `: number`。若稍后尝试赋值 `string`，TypeScript 会报错。

**最佳实践**：让 TypeScript 推断简单的局部变量，但在程序边界（如函数参数和返回值）处显式声明。

## 数组

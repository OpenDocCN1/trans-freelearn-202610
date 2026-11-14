

# 使用 Mocha 进行测试聚合

由于我们必须快速识别哪些测试失败了，因此我们需要一个实用程序来聚合所有测试，并以可读的格式呈现结果。市面上有很多优秀的测试运行器（Karma、Chutzpah、Jasmine 等）。但在本节中，我们将使用 `Mocha`。`Mocha` 非常简单：它会执行我们提供的每个测试，识别通过的测试，然后以显而易见的方式报告任何错误。

### 注意

您可以在 [http://mochajs.org](http://mochajs.org) 找到更多关于 `Mocha` 的文档。

## 安装 Mocha

首先，我们需要安装 `Mocha`：

```
npm install -g mocha
```

## 编写测试套件

接下来，我们需要编写一些 `Mocha` 能够理解的测试套件。请继续删除我们之前创建的 `code.js` 和 `test.js` 文件。现在是进行真正测试的时候了！事实上，让我们来测试一下我们应用的 `AbstractDictionary` 类。

下面是一个非常简单的测试，我编写它是为了验证字典确实能够创建一个新的 `Dictionary` 对象：

```
"use strict";
let should = require("./helpers/setup").should;
import Dictionary from "../src/www/js/app/models/Dictionary";

describe("Dictionary Tests", () => {
    let dictionary = new Dictionary();
    it("should be able to create a new instance", () => {
        return dictionary.should.exist;
    });
});
```

请注意，除了导入我们的 `Dictionary` 对象之外，前导代码与我们之前测试文件中使用的相同。新的部分是以 `describe` 开头的部分。

`describe()` 仅用于将一个或多个测试分组在一起。`describe()` 可以根据需要嵌套任意层级（虽然如果嵌套很深，你可能应该将测试套件拆分到单独的文件中）。在输出中，组内执行的测试会显示一个漂亮的标题，以便于引用。

测试用例是使用 `it()` 方法定义的。为了充分测试你的代码，可以根据需要定义任意数量的 `it()`。事实上，当你想到测试点时，应该将待定测试放入测试套件中；这些测试被定义为没有函数体的 `it()` 方法：

```
it("should be able to jump up and down");     
// 在输出中将标记为 PENDING（待定）
```

这让你稍后可以回来定义实际的测试，同时也能跟踪仍需实际实现的测试。

每个测试可以是完全独立的，也可以依赖于之前的执行结果。这完全取决于你和你正在测试的代码。在我们的例子中，我们希望每个测试都建立在先前的结果之上，以减少代码重复。例如，下面是我们如何验证新创建的字典确实是空的（并且在搜索空字典时不会抛出错误）：

```
describe("Dictionary Tests", () => {
    let dictionary = new Dictionary();
    describe ("#Create", () => {
        it("should be able to create a new instance", () => {
            return dictionary.should.exist;
        });
    });
    describe ("#Empty", () => {
        it("should return an empty sorted index", () => {
            return dictionary.sortedIndex.should.have.lengthOf(0);
        });
        describe ("#find", () => {
            it("searching on an empty dictionary by 
              lemma should not error", () => {
                return dictionary.getEntries({lemma:"cat"}).
                  should.eventually.have.lengthOf(0);
            });
            it("searching on an empty dictionary by wordnetref 
              should not error", () => {
                return dictionary.getEntries({wordNetRef:0}).
                  should.eventually.have.lengthOf(0);
            });
        });
    });
});
```

因为现在有了多个测试，我们引入了额外的 `describe()` 语句。这使我们能够对测试进行分类。第一个测试仅验证我们能否创建一个新字典，因此它归入 `#Create` 类别。接下来的几个测试验证该字典的空性，因此它们共同归入 `#Empty` 类别。然而，最后两个测试在字典内部进行搜索，所以我们给它们一个 `#find` 子类别。在 `Mocha` 的输出中，每个 `describe()` 语句都会创建一个新的缩进层级，因此每个测试的类别都清晰可见。

### 注意

注意前面代码中的 `eventually`？这是 `chai-as-promised` 等待 Promise resolve 的方式。

到目前为止，这些测试都很简单。让我们看一个包含更复杂测试的例子：

```
describe ("#AddApple", () => {
    it("should be able to add an entry.", () => {
        let definition = new Definition({
            wordNetRef: 1,
            lemmas: ["apple"],
            partOfSpeech: "noun",
            gloss: "A tasty fruit"
        });
        return dictionary._addDefinition(definition).should.exist;
    });
    it("should now return a single sorted index", () => {
        return dictionary.sortedIndex.should.have.lengthOf(1);
    });
    it("... that contains the word Apple", () => {
        return dictionary.sortedIndex.
          should.deep.equal(["apple"]);
    });
    describe ("#find", () => {
        it("should be able to find the entry for Apple via lemma", () => {
            return dictionary.getEntries({lemma:"apple"}).should.
                    eventually.have.lengthOf(1).and.
                    deep.property("[0].gloss","A tasty fruit");
        });
        it("should be able to find the entry for Apple via wordnetref", () => {
            return dictionary.getEntries({wordNetRef:1}).should.
                    eventually.have.lengthOf(1).and.
                    deep.property("[0].gloss","A tasty fruit");
        });
    });
});
```

所有这些测试应该都很容易理解。最复杂的测试在 `#find` 类别中。在这些测试中，我们测试 `lengthOf()`（以确保返回结果只包含一项），并确保我们收到了正确的定义。

## 运行测试

定义了一些测试后，我们可以使用 `Mocha` 来运行它们：

```
mocha --require "babel/register" .
```

### 注意

如果你不使用 ES2015，则不需要 `--require "babel/register"` 部分。

该点号指示 `Mocha` 在当前工作目录中运行它能找到的所有测试。你可以指定想要运行的文件，甚至是多个路径和文件。不过，通常人们会希望运行特定目录下的所有测试。

## 解读输出

输出应如下所示：

```
  Dictionary Tests
    #Create
    √ should be able to create a new instance
    #Empty
    √ should return an empty sorted index
      #find
      √ searching on an empty dictionary by lemma should not error
      √ searching on an empty dictionary by wordnetref should not error
    #AddApple
    √ should be able to add an entry.
    √ should now return a single sorted index
    √ ... that contains the word Apple
      #find
      √ should be able to find the entry for Apple via lemma
      √ should be able to find the entry for Apple via 
wordnetref
... [snip] ...

  27 passing (47ms)
```

前面的结果通常是你所期望的：列出的每个测试都已通过（由对勾标记表示），且没有任何错误。虽然这是件好事，但仍然重要的是要记住，这并不能确保你的代码一定没有 Bug：`Mocha` 只能测试你显式定义的情况。

然而，你经常会看到类似以下的输出：

```
  Dictionary Tests

... [snip] ...

    #AddRock/Stone
    √ should be able to add a fourth entry with two lemmas.
    √ should return a sorted index of length 4
      1) ... that is [apple, cat, rock, stone]

... [snip] ...

  26 passing (61ms)
  1 failing

  1) Dictionary Tests #AddRock/Stone ... that is 
[apple, cat, rock, stone] :

      AssertionError: expected ['apple', 'cat', 'rock', 'stone' ] 
to deeply equal ['apple', 'cat', 'rock', 'stones' ]
      + expected - actual
      [
        "apple"
        "cat"
        "rock"
      -  "stone"
      +  "stones"
      ]

... [snipped out stack trace] ...
```


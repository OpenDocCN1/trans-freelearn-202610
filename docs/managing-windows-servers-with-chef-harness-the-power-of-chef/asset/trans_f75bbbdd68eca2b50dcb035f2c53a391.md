

一些组织将所有的 cookbook 都保存在一个仓库中；有些组织则保留多个仓库，每个 cookbook 对应一个。无论你选择哪种版本控制系统和仓库策略，将 cookbook 保存在源代码控制系统中，都是确保配方一致部署的关键部分。在本地进行更改，发出 `knife cookbook upload` 命令，然后忘记提交更改，这非常容易，结果可能会被其他人在稍后日期擦除。此外，在修改 cookbook 时，请确保递增版本号，即使它们仅供内部使用，以便清楚地识别 cookbook 的变化。

如果你计划将配方发布到公共领域，你可能会采用每个仓库一个 cookbook 的方法，使 cookbook 成为一个完整且独立的实体。同样，最好确保包含某种许可证信息（通常以 `LICENSE` 文件的形式）以及某种 `README` 文件，其中包含有关 cookbook 的有用信息、示例用法、如何贡献等等。

## 测试配方

测试配方有多种方法；最简单的方法是将它们安装到 Chef 服务器上，并在测试主机上运行。只要你的客户端运行不会花费过长时间，这是一种可行的方法。然而，也许不出所料，还有更好的选择。其中一种选择是使用名为 `ChefSpec` 的库，它提供了类似于 `RSpec` 的功能，用于测试你的 cookbook。

## RSpec 和 ChefSpec

对于那些没有使用过 `RSpec` 的人来说，它是一个用于测试 Ruby 代码的框架，允许你使用特定领域语言来提供测试。这在概念上类似于 Chef 用于管理基础设施的特定领域语言。`RSpec` 和 `ChefSpec` 提供了一系列语言原语，用于表达代码执行、期望和外部组件的模拟。

## 测试基础

如果你是软件测试的新手，特别是测试 Ruby 代码，这是对我们将要介绍的一些概念的简要介绍。测试可以在软件生命周期的许多不同级别上进行，如下所示：

- 单模块级别（称为单元测试）
- 多模块级别（称为功能测试）
- 系统级测试（也称为集成测试）

在 **测试驱动开发 (TDD)** 哲学中，测试被编写和执行得早且频繁，甚至在编写代码之前。这保证了，假设你的测试是准确的，你的代码从一开始就符合你的期望，并且不会退回到不符合要求的先前状态。本节不会深入探讨 TDD 哲学和持续测试，而是为你提供足够的知识，以开始测试你编写的配方，并对它们在部署到生产环境时能做正确的事情感到有信心。

## RSpec

如前所述，RSpec 旨在提供一种更具表现力的测试语言。这意味着所提供的方法和原语被有意地设计为协同工作，以构建一种更高级的语言，感觉更像自然语言，例如英语。例如，使用 RSpec，可以编写以下代码：

`factorial(4).should eq 24`

如果你读一下，感觉就像“四的阶乘应该等于 24”。这是 RSpec 的 DSL 的目标，在阅读测试时感觉自然。将其与类似的 JUnit 测试（用于 Java）进行比较，如下所示：

`assertEquals(24, factorial(4));`

虽然大多数程序员都能读懂，但它不那么自然。除此之外，RSpec 的 DSL 具有上下文和活动块，使测试更易于阅读。例如，我们可以使用 RSpec 如下：

`describe Array do`
`  it "should be empty when created" do`
`    Array.new.should == []`
`  end`
`end`

同样，与类似的 NUnit (.NET 示例) 相比：

`namespace MyTest {`
`  using System.Collection`
`  using NUnit.Framework;`
`  [TestFixture]`
`  public class ArrayTest {`
`    [Test]`
`    public void NewArray() {`
`      ArrayList list = new ArrayList();`
`      Assert.AreEqual(0, list.size());`
`    }`
`  }`
`}`

很明显，RSpec 测试要简洁得多，也更易于阅读，这是 RSpec 的目标。ChefSpec 测试将 RSpec 的表现力带到 Chef cookbook 和配方中，通过在 RSpec 简单的测试语言之上提供 Chef 特定的原语和机制。

## ChefSpec

ChefSpec，对于那些没有使用过它的人来说，是任何 Chef 开发者工具集中的一个绝佳补充。它易于使用，并提供了一种方便的方式来为你的 cookbook 和配方添加测试。

## 开始使用 ChefSpec

为了开始使用 ChefSpec，你需要安装一个包含 ChefSpec 库和所有支持组件的 gem。不出所料，该 gem 名为 chefspec，可以通过运行以下命令简单安装：

`gem install chefspec`

然而，由于正在安装许多其他依赖项，这里有一个 Gemfile，用于将 gem 锁定到编写这些示例时使用的版本：

```
source 'https://rubygems.org'

gem 'chef',
    '11.10.0'
gem 'chefspec',
    '3.2.0'
gem 'colorize',
    '0.6.0'
```

为了使用 Gemfile，你需要安装 bundler；如果你使用 RVM，bundler 应该随你创建的每个 gem 集一起安装。如果没有，你需要自己通过键入以下代码行来安装它：

`gem install bundler`

一旦 bundler 安装完成，并且包含上述代码行的 Gemfile 放置在你的 cookbook 根目录中，你可以从 cookbook 目录内执行以下命令：

`bundle install`

Bundler 将解析 Gemfile 以下载和安装 chefspec、chef 和 colorize，以及你尚未拥有的任何依赖项。安装完成后，你将希望在 cookbook 内创建一个 spec 目录，并创建一个 default_spec.rb 文件。spec 文件的名称应与配方文件的名称匹配，因此如果你有一个名为 default.rb 的配方文件（大多数 cookbook 都会有），你将把 spec 文件命名为 default_spec.rb。让我们来看看一个非常简单的配方和匹配的 ChefSpec 测试。

如这里定义的配方，将简单地在目标主机上创建一个位于 /tmp/myfile.txt 的文件，如下所示：

```
file "/tmp/myfile.txt" do
  owner "root"
  group "root"
  mode "0755"
  action :create
end
```

如以下代码片段所示的匹配测试，将验证我们的 cookbook 是否会按我们期望的那样工作：

```
require 'chefspec'

describe 'demo_cookbook::default' do
  let(:chef_run) {
    ChefSpec::Runner.new.converge(described_recipe)
  }

  it 'creates a file' do
    expect(chef_run).to create_file('/tmp/myfile.txt')
  end
end
```

## 执行测试

现在，为了运行它，我们使用 `rspec` 命令，它将使用 RSpec 语言将测试作为 Ruby 脚本运行。在这里，它还将使用 ChefSpec 扩展，因为在我们的 spec 测试中，我们在顶部有 `require 'chefspec'` 行来包含 ChefSpec。以下示例使用 bundler 运行 RSpec，以便在执行前加载所有必需的 Ruby gem：

`bundle exec rspec spec/default_spec.rb`

这将使用 bundler 运行 RSpec，它将确保加载 Gemfile 中指定的正确版本的 gem，并处理 `default_spec.rb` 文件。运行后，你应该看到测试结果，它们应该类似于以下输出：

```
Finished in 0.17367 seconds
1 example, 0 failures
```

这是 RSpec 告诉你它已完成，并且你有一个测试，零个失败。然而，如果我们有一个失败的测试，结果就会大不相同；RSpec 会告诉我们哪个测试失败了以及原因。

## 理解失败

RSpec 非常擅长告诉你测试中出了什么问题；如果无法确定出了什么问题，有失败的测试对你也没有好处。当测试中的期望未得到满足时，RSpec 会告诉你哪个期望未得到满足以及意外的值是什么。例如，让我们从以下代码中获取我们的演示配方文件资源：

```
file "/tmp/myfile.txt" do
```

让我们将其替换为不同的文件名，例如 myfile2.txt 而不是 myfile.txt，如以下代码所示：

```
file "/tmp/myfile2.txt" do
```

现在，如果我们重新运行 spec 测试，我们会发现测试现在失败了，因为 Chef 做了一些 spec 测试未预期的事情，如以下示例所示：

```
[user@host]$ bundle exec rspec spec/default_spec.rb
F

Failures:

1) demo_cookbook::default creates a file
   Failure/Error: expect(chef_run).to create_file('/tmp/myfile.txt')
     expected "file[/tmp/myfile.txt]" with action :create to be in Chef
   run. Other file resources:

   file[/tmp/myfile2.txt]

   # ./spec/default_spec.rb:9:in `block (2 levels) in <top (required)>'

Finished in 0.18071 seconds
1 example, 1 failure
```

在前面的示例中，你可以看到 RSpec（与 ChefSpec 配合使用）正在告诉我们，'demo_cookbook::default' 测试套件中的 'creates a file' 测试失败了。它还告诉我们，它在 default_spec.rb 的第 9 行失败了（由包含 ./spec/default_spec.rb:9 的行指示），因为它期望文件资源 /tmp/myfile.txt 被 :create 操作交互，但相反，配方与一个文件资源 /tmp/myfile2.txt 进行了交互。


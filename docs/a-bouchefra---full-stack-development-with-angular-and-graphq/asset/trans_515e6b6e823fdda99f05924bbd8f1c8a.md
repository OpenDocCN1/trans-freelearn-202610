
```
margin: 0.4rem;
}
.profile-cover-card {
    margin: 0px;
    padding: 0px;
}
```

这些样式用于将 box-sizing 设置为 border-box，这指示浏览器在计算我们赋予元素的宽度和高度值时，将边框和内边距纳入考虑。

接下来，我们为 Material Card 元素设置外边距，并将包含个人资料封面图像和用户照片的卡片的外边距和内边距均设置为 0 像素。

- 2. 按如下方式设置具有 .profile-cover 类的元素（包含封面图像的 <div> 元素）：

```
.profile-cover {
    height: 10rem;
    width: 100%;
    background-position: center;
    background-repeat: no-repeat;
    background-size: cover;
    background-color:rgb(97, 76, 76);
    position: absolute;
}
```

为了占据页面的整个宽度，我们将宽度设置为 100%，然后将高度设置为 10rem。随后将背景属性设置为位置居中，重复属性为不重复。最重要的是，我们将 background-size 属性设置为 cover 并使用绝对定位。

查看 https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Backgrounds_and_Borders/Resizing_background_images。

3. 添加用户图像的以下样式：

```css
.profile-image {
  width: 9rem;
  height: 9rem;
  clip-path: circle(60px at center);
  background-color: gray;
  border-width: 1px;
  margin-top: 5rem;
}
```

在这种情况下，我们将宽度和高度均设置为 9rem，使用 clip-path 属性将图像裁剪为圆形，并为用户头像设置默认背景，当用户未上传照片时将显示该背景。

查看 https://developer.mozilla.org/en-US/docs/Web/CSS/clip-path。

4. 接下来，让我们使用 CSS Grid 布局设置元素的样式：

```css
.profile-grid {
  display: grid;
  grid-template-columns: 1fr 2.5fr 1.5fr;
}
.profile-bio-btn {
  margin: 0.4rem;
  width: 100%;
}
.profile-bio-box {
  width: 100%;
  height: 5rem;
}
.bio-box-wrapper > textarea {
  padding: 10px;
  background-color: #f0f2f5;
  border-radius: 9px;
}
```

profile-grid 类包含封面图像和用户照片下方的所有信息，以及包含全名、简介、日期、帖子计数和帖子的元素。

我们将显示切换到网格模式，并使用 fr 单位（分数单位）将可用水平空间划分为比例为 1、2.5 和 1.5 的三列。查看 https://www.techiediaries.com/css-grid-layout-tutorial/ 获取详细说明。

5. 设置在找不到与对应个人资料用户 ID 匹配的用户时将显示的元素样式：

```
.user-not-found {
    text-align: center;
    font-size: 5vw;
    margin: 100px;
}
```

6. 对于最大宽度为 600px 的小屏幕，添加以下样式：

```
@media (max-width: 600px) {
    .profile-grid {
        display: grid;
        grid-template-columns: 1fr;
    }
}
```

如果屏幕宽度小于 600px，我们仅为个人资料网格元素使用一列，该列占据所有可用水平空间。

在添加这些样式后，在创建账户并添加封面和照片图像后，您将拥有如下图所示的个人资料：

![](img/c798729e4c42765fcb1c98950164d0ba_298_0.png)

图 9.1 – 个人资料组件的顶部部分

以下是点击 Edit.. 按钮后用户简介部分的屏幕截图：

![](img/c798729e4c42765fcb1c98950164d0ba_299_0.png)

图 9.2 – 用户简介部分

以下是添加简介后用户简介部分的屏幕截图：

![](img/c798729e4c42765fcb1c98950164d0ba_299_1.png)

图 9.3 – 用户信息部分

现在我们已经完成了个人资料组件中用户信息的实现，让我们继续进行用户帖子部分。

## 实现创建帖子组件

在本节中，我们将实现创建帖子组件，该组件将用于从个人资料页面（以及主页信息流）创建帖子。我们将创建帖子的逻辑放在自己的可重用组件中，因为它将在多个页面上需要。

此组件被称为 **presentational** 或 **dumb** 组件。它接收输入并向父组件发出自定义输出事件。此类组件不知道如何获取数据（没有注入的服务），因此其唯一职责是接收数据、渲染它，并为父组件处理发出一些事件。在我们的例子中，这些父组件是个人资料和信息流的帖子组件，它们被称为 **container** 或 **smart** 组件。

当此组件的 post 事件触发时，我们之前定义的基组件的 onPost() 方法将被调用。当用户点击创建帖子的按钮时，将触发此自定义事件，我们将在以下步骤中实现该功能：

- 1. 首先通过在 client/ 文件夹中运行以下命令生成一个组件：
- 2. 按如下方式从 shared/shared.module.ts 文件导出 create-post 组件：
- 3. 首先在 core/components/create-post/ 文件夹中的 create-post.component.ts 文件中添加以下导入：

```
ng g c core/components/create-post --module=shared
```

```
@NgModule({
  declarations: [CreatePostComponent],
  imports: [
    CommonModule, ReactiveFormsModule, ...matModules
  ],
  exports: [ReactiveFormsModule, ...matModules,
    CreatePostComponent]
})
```

```
import {
  Component,
  ElementRef,
  EventEmitter,
  Input,
}
OnInit,
Output,
ViewChild
} from '@angular/core';
import { User }
from '@ngsocial/graphql/types';
import { PostEvent }
from 'src/app/shared';
```

我们导入 Angular 核心模块的 API、之前定义的 User 类型和 PostEvent。

4. 按如下方式定义组件的公共和私有属性：

```
@Input() authUser!: Partial<User>;
@Input() loading: boolean = false;
@Output() post: EventEmitter<PostEvent> = new
EventEmitter();
public imageFile: File | null = null;
@ViewChild('postText') postText!: ElementRef;
```

我们添加两个自定义输入属性用于通过父组件接收已认证用户和加载状态，以及一个自定义事件，该事件将在用户点击创建帖子按钮时触发。

使用输入属性和事件，我们确保组件之间的父子通信。有关更多信息，请参阅 https://angular.io/guide/inputs-outputs#sending-data-to-a-child-component。

我们使用 @Input 装饰器装饰类属性以创建输入属性，我们可以通过创建 EventEmitter 的实例并将其作为属性公开，该属性用 @Output 装饰，父组件可以绑定到它以监听自定义事件并通过 $event 对象访问相关数据。

如果我们不使用默认值初始化所有定义的组件属性，TypeScript 将抛出错误。这是因为 Angular 12+ 默认启用了 strict 模式的 strictPropertyInitialization 规则。

我们还定义了 `imageFile` 属性，如果存在则包含用户选择的图像，以及 `postText` 属性，我们用 `ViewChild` 装饰它，以便让我们在相关模板中获取由 `#postText` 模板引用引用的输入元素的文本值：

1. 定义以下 getter 以获取已认证用户的名字：

```
get userFirstName() {
    return this.authUser
        ?.fullName
        ?.split(' ')
        ?.shift();
}
```

2. 定义以下方法以获取用户选择的图像：

```
onFileSelected(event: Event): void {
    const files: FileList =
        (event.target as HTMLInputElement)
        .files!;
    if (files.length > 0) {
        this.imageFile = files[0];
    }
}
```

此方法将绑定到用于从用户计算机选择图像的输入字段的 `change` 事件。图像文件信息从事件中提取并存储在 `imageFile` 属性中。

3. 定义以下方法以处理创建帖子的按钮点击事件：

```
handlePostClick(): void {
    if (this.authUser!.id) {
        this.post.emit({
            text: this.postText?.nativeElement.value,
            image: this.imageFile as File
        });
    }
    if (this.postText.nativeElement) {
        this.postText.nativeElement.value = '';
    }
}
```

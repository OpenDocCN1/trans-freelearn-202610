# 约定

在本书中，你将发现许多文本样式，用于区分不同种类的信息。以下是这些样式的一些示例及其含义的解释。

文本中的代码词、数据库表名、文件夹名、文件名、文件扩展名、路径名、虚拟 URL、用户输入和 Twitter 账号显示如下："Finally, we will need to implement the `GameLogic` class."

代码块显示如下：

```
void Start() {
    GameLogic.Instance.RegisterUpdateableObject(this);
    Initialize();
}
```

```
protected virtual void Initialize() {
    // derived classes should override this method for initialization code,
    and NOT reimplement Start()
}
```

**新术语**和**重要词汇**以粗体显示。屏幕上看到的单词，例如在菜单或对话框中，在文本中显示如下："Select **System info** from the **Administration** panel."

警告或重要说明显示在类似这样的框中。

技巧和窍门显示如下。



## 如何操作...

首先，我们创建一个新的Python模块，在其中放置两个新类。第一个类非常简单，用于定义日志级别。这基本上是一个枚举：

```python
class LogLevel:
    '''Define logging levels.'''
    OFF      = 0
    MINIMUM  = 1
    NORMAL   = 2
    DEBUG    = 3
```

第二个类通过传入的文件名的完整路径创建一个日志文件，并将其放入`logs`文件夹中。首次运行时，`logs`文件夹可能不存在，因此代码会自动创建该文件夹：

```python
import os, time
from datetime import datetime

class Logger:
    ''' Create a test log and write to it. '''

    #------------------------------------------------------------
    def __init__(self, fullTestName, loglevel=LogLevel.DEBUG):
        testName = os.path.splitext(os.path.basename(fullTestName))[0]
        logName  = testName + '.log'

        logsFolder = 'logs'
        if not os.path.exists(logsFolder):
            os.makedirs(logsFolder, exist_ok=True)

        self.log = os.path.join(logsFolder, logName)
        self.createLog()

        self.loggingLevel = loglevel
        self.startTime    = time.perf_counter()

    #------------------------------------------------------------
    def createLog(self):
        with open(self.log, mode='w', encoding='utf-8') as logFile:
            logFile.write(self.getDateTime() +
                          '\t\t*** Starting Test ***\n')
        logFile.close()
```

为了写入日志文件，我们使用`writeToLog()`方法。在该方法内部，我们首先检查消息的日志级别是否高于我们设置的所需日志输出级别。如果消息的级别较低，我们就丢弃它并从方法中立即返回。

如果消息具有我们想要显示的日志级别，我们接着检查它是否以换行符开头。如果是，我们使用Python的切片运算符从索引1开始切片来丢弃换行符（`msg = msg[1:]`）。

然后我们向日志文件写入一行，内容为当前日期时间戳、两个制表符空格、我们的消息，并以换行符结束：

```python
#--------------------------------------------------
def writeToLog(self, msg='', loglevel=LogLevel.DEBUG):
    # control how much gets logged
    if loglevel > self.loggingLevel:
        return

    # open log file in append mode
    with open(self.log, mode='a', encoding='utf-8') as logfile:
        msg = str(msg)
        if msg.startswith('\n'):
            msg = msg[1:]
        logfile.write(self.getDateTime() + '\t\t' + msg + '\n')

    logfile.close()
```

我们现在可以导入新的Python模块，并在GUI代码的`__init__`部分中创建`Logger`类的实例：

```python
from os import path
from Ch08_Code.Logger import Logger

class OOP():
    def __init__(self):
        # create Logger instance
        fullPath = path.realpath(__file__)
        self.log = Logger(fullPath)
        print(self.log)
```

我们通过`path.realpath(__file__)`获取正在运行的GUI脚本的完整路径，并将其传递给`Logger`类的初始化器。如果`logs`文件夹不存在，我们的Python代码会自动创建它。

这将产生以下结果：

`Callbacks_Refactored.py`，取消注释`print(self.log)`

![](img/986b0dbf3f539f4f3baa82bd3003f04e_311_0.png)

前面的截图显示我们创建了新的`Logger`类的一个实例，下面的截图显示`logs`文件夹和日志都已创建：

![](img/986b0dbf3f539f4f3baa82bd3003f04e_311_1.png)

当我们打开日志时，可以看到当前日期和时间以及默认字符串已被写入日志：

![](img/986b0dbf3f539f4f3baa82bd3003f04e_311_2.png)

## 工作原理...

在本节中，我们创建了自己的日志类。虽然Python自带一个日志模块，但自己创建非常简单，这让我们能够完全控制日志格式。当我们将自己的日志输出与MS Excel或我们在上一章中探索的Matplotlib结合使用时，这非常有用。

在下一节中，我们将使用Python内置的`__main__`功能来使用我们刚刚创建的四个不同日志级别。

## 使用Python的`__main__`段创建自测试代码

Python带有一个非常好的特性，使每个模块都能进行自测试。利用该特性是确保我们对代码的修改不会破坏现有代码的好方法，此外，`__main__`自测试部分还可以作为每个模块如何工作的文档。

> 几个月或几年后，我们有时会忘记代码在做什么，因此在代码本身中写有解释确实非常有益。

尽可能为每个Python模块添加自测试部分是一个好主意。有时这不可能，但在大多数模块中是可以做到的。

### 准备工作

我们将扩展上一节的内容，因此为了理解本节中代码的作用，我们必须首先阅读并理解上一节的代码。

### 如何操作…

首先，我们通过将自测试部分添加到`LanguageResources.py`模块中来探索Python `__main__`自测试部分的强大功能。每当我们运行一个模块时，如果该模块底部有自测试部分，当该模块单独执行时，这些代码将运行。

当模块被其他模块导入和使用时，`__main__`自测试部分中的代码不会被执行。

这也是以下截图中显示的代码：

```python
if __name__ == '__main__':
    language = 'en'
    inst = I18N(language)
    print(inst.title)

    language = 'de'
    inst = I18N(language)
    print(inst.title)
```

添加自测试部分后，我们现在可以单独运行该模块，它会生成有用的输出，同时向我们展示代码按预期工作：

```text
LanguageResources.py
```

![](img/986b0dbf3f539f4f3baa82bd3003f04e_313_0.png)

我们首先将英语作为GUI中显示的语言传入，然后将德语作为GUI将显示的语言传入。

我们打印GUI的标题，以表明我们的Python模块按我们预期的方式工作。

![](img/986b0dbf3f539f4f3baa82bd3003f04e_313_1.png)

下一步是使用我们在上一节中创建的日志功能。

我们首先在重构后的`GUI_Refactored.py`模块中添加一个`__main__`自测试部分，然后验证我们创建了`Logger`类的实例：

`GUI_Refactored.py`，取消注释`print(oop.log)`

![](img/986b0dbf3f539f4f3baa82bd3003f04e_314_0.png)

接下来，我们使用所示的命令写入日志文件。我们将日志级别设计为默认记录所有消息，即DEBUG级别，因此我们无需更改任何内容。只需将需要记录的消息传递给`writeToLog`方法：

```python
if __name__ == '__main__':
    #===================
    # Start GUI
    #===================
    oop = OOP()
    print(oop.log)
    oop.log.writeToLog('Test message')
    oop.win.mainloop()
```

这将被写入我们的日志文件，从下面的日志截图中可以看到：

![](img/986b0dbf3f539f4f3baa82bd3003f04e_314_1.png)

现在，我们可以通过为日志语句添加日志级别并设置我们想要输出的级别来控制日志记录。让我们将此功能添加到`Callbacks_Refactored.py`模块中的**纽约**按钮回调方法中，即`getDateTime`方法。

我们将之前的`print`语句改为使用不同调试级别的`log`语句。

在`GUI_Refactored.py`中，我们从`Logger`模块导入两个新类：

```python
from Ch08_Code.Logger import Logger, LogLevel
```

接下来，我们创建这些类的本地实例：

```python
# create Logger instance
fullPath = path.realpath(__file__)
self.log = Logger(fullPath)

# create Log Level instance
self.level = LogLevel()
```

由于我们将GUI类的实例传递给`Callbacks_Refactored.py`的初始化器，我们可以根据我们创建的`LogLevel`类来使用日志级别限制：

```python
# Format local US time with TimeZone info
def getDateTime(self):
    fmtStrZone = "%Y-%m-%d %H:%M:%S %Z"
    # Get Coordinated Universal Time
    utc = datetime.now(timezone('UTC'))
    self.oop.log.writeToLog(utc.strftime(fmtStrZone),
                            self.oop.level.MINIMUM)

    # Convert UTC datetime object to Los Angeles TimeZone
    la = utc.astimezone(timezone('America/Los_Angeles'))
    self.oop.log.writeToLog(la.strftime(fmtStrZone),
                            self.oop.level.NORMAL)

    # Convert UTC datetime object to New York TimeZone
    ny = utc.astimezone(timezone('America/New_York'))
    self.oop.log.writeToLog(ny.strftime(fmtStrZone),
                            self.oop.level.DEBUG)

    # update GUI label with NY Time and Zone
    self.oop.lbl2.set(ny.strftime(fmtStrZone))
```


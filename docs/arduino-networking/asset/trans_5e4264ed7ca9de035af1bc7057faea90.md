

```
if (temperature > 23) {
    Serial.println("\nTriggered! Calling /Library/Google/Gmail/SendEmail...");

    // Send email
    runSendEmail(temperature);
}
```

你可以看到，每当温度超过该限制时，就会通过 Temboo 发送一封电子邮件警报。我们稍后将详细介绍此函数。我在每次测量之间还插入了 1 分钟的延迟，因此如果跨越阈值，我们不会持续发送电子邮件：

```
delay(60000);
```

现在让我们来看看 runSendEmail() 函数的详细信息。该函数负责创建用于发送电子邮件的正确 Choreo，设置电子邮件正文，并通过 Temboo 发送。

它首先通过以下行创建用于发送电子邮件的正确 Choreo：

```
TembooChoreoSendEmailChoreo(client);
```

我们还定义了与你的账户相关的不同信息，这些信息在你刚刚从 Temboo 下载的 Temboo 账户文件中定义：

```
SendEmailChoreo.setAccountName(TEMBOO_ACCOUNT);
SendEmailChoreo.setAppKeyName(TEMBOO_APP_KEY_NAME);
SendEmailChoreo.setAppKey(TEMBOO_APP_KEY);
```

接下来，你需要在此处输入你刚刚在 Temboo 上创建的电子邮件项目的名称。如果你使用与我相同的名称，则无需修改以下行中的任何内容：

```
SendEmailChoreo.setSavedInputs("arduinoTemperatureEmail");
```

我们现在将构建电子邮件正文，它将存储在字符串变量中。我们只是使用一条通用消息，将测量温度的值附加其中，并将所有内容存储在一个字符串中。最后，我们将把以下行添加为用于发送电子邮件的 Choreo 的输入：

```
String MessageBodyValue = String("Alert! The current sensor value is: ") +
String(sensorValue);
SendEmailChoreo.addInput("MessageBody", MessageBodyValue);
```

在执行 Choreo 之前，我们定义使用以下行从 Gmail 使用 SendEmailChoreo：

```
SendEmailChoreo.setChoreo("/Library/Google/Gmail/SendEmail");
```

我们最终可以执行 Choreo 并将结果存储在变量中：

```
unsigned int returnCode = SendEmailChoreo.run();
```

与上一节一样，如果结果为 0，则表示一切正常。如果不是这种情况，我们将使用以下代码片段在串行端口上打印错误消息以进行调试：

```
if (returnCode == 0) {
  Serial.println("Done!\n");
} else {
  // A non-zero return code means there was an error
  // Read and print the error message
  while (SendEmailChoreo.available()) {
    char c = SendEmailChoreo.read();
    Serial.print(c);
  }
  Serial.println();
}
```

最后，我们将使用以下行关闭 Choreo：

```
SendEmailChoreo.close();
```

## 注释

你可以在本章的 GitHub 仓库中找到本节的所有代码：https://github.com/openhomeautomation/arduino-networking/tree/MASTER/chapter6。

你现在可以测试草图了。首先要确保你的 Temboo 账户文件与 Arduino 草图在同一文件夹中。然后，将草图上传到 Arduino 开发板并打开串行监视器。你首先应该看到的将是一切正常的确认消息：

```
**DHCP: OK**
**Setup complete**
```

接着，你应该在串行监视器上看到首次测量结果：

```
**Temperature: 21**
**Humidity: 39**
```

根据温度值，你可能需要捏住传感器来触发电子邮件警报。请记住，在本章示例中，我们将温度设置为 23 度。过一会儿，如果温度达到阈值，你应该看到以下消息：

```
**Triggered! Calling /Library/Google/Gmail/SendEmail...**
```

过一会儿，应该出现以下确认消息：

```
**Done!**
```

你现在可以关闭串行监视器，去检查你在 Temboo 上设置的电子邮件账户。你应该已经收到一条带有你在 Temboo 上设置的标题的消息，你还会收到正文，该正文指示你在草图中设置的通用消息以及测得的温度。

## 摘要

在本章中，我们使用网络服务 Temboo 创建了另一个物联网项目。基于 Temboo，我们构建了两个令人兴奋的应用程序。第一个是将测量值自动记录到 Google Docs 电子表格中。与我们在上一章中采用的方法相比，这里的优势很明显，即 Google Docs 是创建和共享电子表格的广泛使用的标准。你只需使用网页浏览器就可以随时随地访问数据，还可以绘制数据图表，并轻松与朋友和家人共享。

我们还构建了一个令人兴奋的应用程序，通过 Temboo 将我们的项目与 Gmail 对接。我们构建了一个基于测量数据的自动电子邮件警报系统。

让我们看看本章的主要收获是什么。首先，我们构建了项目的硬件，在 Arduino 以太网扩展板上安装了 DHT11 温湿度传感器。

接着，我们设置了项目所需的不同账户。我们首先在 Google Docs 上创建了一个电子表格，并正确配置它，以便它可以与 Temboo 协同工作。然后，我们还创建了一个 Temboo 账户并对其进行配置，以便你的以太网扩展板可以将数据发送到 Temboo 并将测量值记录到 Google Docs。

设置阶段完成后，我们构建了该项目的第一个应用程序，该应用程序包括将数据直接记录到 Google Docs 电子表格中。我们还使用 Google Docs 的绘图功能显示了这些数据。

最后，我们再次使用 Temboo 创建了一个自动电子邮件警报系统，当测量值达到特定阈值时，它会自动向你选择的地址发送一封电子邮件。

由于本书即将结束，让我们来看看我们了解了关于 Arduino 以太网扩展板的哪些内容，以及可以用它创建哪些令人兴奋的应用程序。

本书的第一部分主要关于在以太网扩展板上运行 Web 客户端。我们首先尝试了扩展板，看看它是否能连接到互联网并获取页面内容。然后，我们构建了第一个测量系统，其中以太网扩展板连接到运行在你计算机上的服务器以记录一些数据。

在本书的第二部分，我们通过在 Arduino 开发板上直接运行 Web 服务器，为 Arduino 以太网扩展板赋予了更多自主权。我们再次制作了测量项目，但这次是在开发板上运行 Web 服务器，并将数据记录到插入扩展板的 SD 卡中。我们还将继电器连接到 Arduino 开发板，并通过以太网对其进行控制。

最后，我们使用 Xively 和 Temboo 平台构建了两个物联网项目。在这两个项目中，我们都使用以太网扩展板将数据发送到 Web，以便这些数据可以从世界任何地方访问并以图形方式显示。

为本书画上句号，我希望你在不同章节中找到的所有信息都能激发你构建更多令人兴奋的应用程序的意愿。Arduino 以太网扩展板是使用 Arduino 构建精彩联网应用程序的最佳方式之一，而你用本书所学概念能做的事情也没有任何限制。

## 索引

### A

- 账户，Google
  - 设置 / Setting up your Google Account
- 账户，Temboo
  - 创建 / Creating your Temboo account
- 应用程序，使用 Arduino 开发板构建
  - 硬件要求 / Hardware and software requirements
  - 软件要求 / Hardware and software requirements
  - 硬件配置 / Hardware configuration
- 应用程序，使用 Arduino 以太网扩展板构建
  - 硬件要求 / Hardware and software requirements
  - 软件要求 / Hardware and software requirements
  - 硬件配置 / Hardware configuration
- Arduino 开发板
  - 在 Arduino 上创建 Web 服务器 / Creating a web server on Arduino
- Arduino 以太网扩展板
  - URL / 硬件和软件要求, 硬件和软件要求
- Arduino IDE
  - URL / 硬件和软件要求
- Arduino 库
  - 安装 / 硬件和软件要求
- Arduino Temboo 库
  - URL / 硬件和软件要求
  - 安装 / 硬件和软件要求
- Arduino Uno
  - URL / 硬件和软件要求, 硬件和软件要求
- 自动电子邮件
  - 发送 / Sending automated e-mails


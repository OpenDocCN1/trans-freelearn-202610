
On the other hand, if the first parameter is a number prefixed with a hash, it is not ignored. In that case, it works like an nth value specifier. For example, last (#9) would retrieve the 9th most recent value. As we can see, last (#1) is equal to last (0) or last (). Another overlapping function is prev. As the name might suggest, it returns the previous value; thus, prev() is the same as last (#2).

> Hostname, item key, trigger function, operators — they all are case sensitive.

Continuing with the trigger expression, curly braces are closed to represent a string that retrieves some value, that is, host and item reference, followed by the trigger function. Then we have an operator, which in this case is a simple equals sign. The comparison here is done with a constant number, 0.

> If item history is set to 0, no values are stored and no triggers are evaluated, even if those triggers would only check the last value. This is different from the previous versions of Zabbix, where triggers, referencing the last value only, would still work.

## The trigger-and-item relationship

You might have noticed how items in Zabbix do not contain any configuration on the quality of the data — if the CPU load values arrive, the item does not care whether they are 0 or 500. Any definition of a problem condition happens in a trigger, whether it's a simple threshold or something more complex.

And when we created this trigger, we could click on any of the Triggers links, but we paid attention to the host selected in the dropdowns when choosing the item. It actually does not matter which of those Triggers links we click on, as long as the proper host is selected in that popup or we manually enter the correct hostname.

> A trigger does not belong to a host like an item does. A trigger is associated with any number of hosts it references items from.

If we clicked on Triggers for host A and then chose an item from host B for that trigger, the created trigger would not appear for host A, but would appear for host B.

![](img/f5313a804532fabacc8d07cc8607eeba_225_0.png)

This inserts the `{Another Host:net.tcp.service[http,,80].last()}=0` expression:

- **Severity:** Average
- **Description:** Trigger expressions can get very complex. Sometimes, the complexity can make it impossible to understand what a trigger is supposed to do without serious dissection. Comments provide a way to help somebody else, or yourself, understand the thinking behind such complex expressions later. While our trigger is still very simple, we might want to explain the reason for the dependency, so enter something such as `Web service goes down if SMTP is inaccessible.`

Now, switch to the **Dependencies** tab. To configure the dependency of the web frontend on the SMTP service, click on the **Add** link in the **Dependencies** section. In the resulting window, make sure **Linux servers** is selected in the **Group** dropdown and **Another host** is selected in the **Host** dropdown, and then click on the only entry in the **NAME** column: **SMTP service is down.**

![](img/f5313a804532fabacc8d07cc8607eeba_225_1.png)

## 第一章 人工智能概述

人工智能（Artificial Intelligence，简称 AI）是研究、开发用于模拟、延伸和扩展人的智能的理论、方法、技术及应用系统的一门新的技术科学。

人工智能是计算机科学的一个分支，它企图了解智能的实质，并生产出一种新的能以人类智能相似的方式做出反应的智能机器，该领域的研究包括机器人、语言识别、图像识别、自然语言处理和专家系统等。

- 人工智能的发展历史
- 人工智能的主要研究领域
- 人工智能的应用场景

> “人工智能就是关于知识的科学——怎样表示知识、怎样获取知识和怎样使用知识。”

| 年代 | 标志性事件 |
| --- | --- |
| 1950 | 图灵测试提出 |
| 1956 | 达特茅斯会议 |
| 1997 | 深蓝击败卡斯帕罗夫 |
| 2016 | AlphaGo击败李世石 |

![](img/f5313a804532fabacc8d07cc8607eeba_226_0.png)

```
```python
import ai
model = ai.load_model('gpt')
result = model.predict('你好')
```
```

本章介绍了人工智能的基本概念和发展脉络，后续章节将深入探讨各类算法及其应用。

![](img/f5313a804532fabacc8d07cc8607eeba_227_0.png)

A popup appears, informing us that there are other triggers dependent on this one. Dependent triggers are listed in the popup. Now, move the mouse cursor on one cell below, over the downward-pointing arrow:

![](img/f5313a804532fabacc8d07cc8607eeba_227_1.png)

Let's see what effect, other than the arrows, this provides. Go to **Monitoring | Triggers**, and make sure both **Host** and **Group** dropdowns say **all**. Then, bring down the web server on **Another host**. Wait for the trigger to fire, and look at the entry. Notice how an arrow indicating dependency is displayed here as well. Move the mouse cursor over it again, and the dependency details will be displayed in a popup:

| SEVERITY | STATUS | INFO | LAST CHANGE | AGE | ACK | HOST | NAME | DESCRIPTION |
|---|---|---|---|---|---|---|---|---|
| Average |  |  | 2016-04-05 15:49:01 | 3s | No | Another host | Web service is down | Show |

But what's up with the Show link in the DESCRIPTION column? Let's find out; click on it. The description we provided when creating the trigger is displayed. This allows easy access to descriptions from the trigger list, both for finding out more information about the trigger and updating the description. Click on cancel ( × ) to return to the trigger list. Now, stop the SMTP service on Another host. Wait for the trigger list to update, and look at it again. The web server trigger has disappeared from the list and is replaced by the SMTP server one. That's because Zabbix does not show dependent triggers if the dependency upstream trigger is in the PROBLEM state. This helps keep the list short and concentrate on the problems that actually cause the downtime.

| SEVERITY | STATUS | INFO | LAST CHANGE ▼ | AGE | ACK | HOST | NAME | DESCRIPTION |
| Average | PROBLEM |  | 2016-04-05 15:51:00 | 3s | No | Another host | SMTP service is down | Add |
|  |  |  |  |  |  |  | Dependent: Web service is down |  |

Trigger dependencies are not limited to a single level. We will now add another trigger to the mix. Before we do that, we'll also create an item that will provide an easy way to manually change the trigger state without affecting system services. In the frontend, navigate to Configuration | Hosts, click on Items next to Another host, and then click on Create item. Fill in the following values:

- Name: Testfile exists
- Key: vfs.file.exists[/tmp/testfile]

When you are done, click on the Add button at the bottom. As the key might reveal, this item simply checks whether a particular file exists and returns 1 if it does, and 0 if it does not.

> Using a constant filename in /tmp in real-life situations might not be desirable, as any user could create such a file.

In the bar above the Item list, click on Triggers, and then click on the Create trigger button. Enter these values:

- Name: Testfile is missing.
- Expression: Click on Add and then on Select next to the Item field. In the item list for Another host, click on Testfile exists in the NAME column, and then click on Insert (again, the default function works for us). The Expression field is filled with the following expression:
  {Another Host:vfs.file.exists[/tmp/testfile].last()}=0
- Severity: Warning.

This should deal with the only trigger currently on the trigger list. Wait for the trigger list to update. You will see two triggers, with their statuses flashing. Remember that, by default, Zabbix shows triggers that have recently changed state as flashing, and that also includes triggers in the "OK" state:

| SEVERITY | STATUS | INFO | LAST CHANGE ▼ | AGE | ACK | HOST | NAME | DESCRIPTION |
|---|---|---|---|---|---|---|---|---|
| Warning | OK |  | 2016-04-05 16:12:25 | 1m 4s | No 1 | Another host | ↑ Testfile is missing | Add |
| Average | PROBLEM |  | 2016-04-05 15:51:00 | 22m 29s | No 8 | Another host | ↑ ↓ SMTP service is down | Add |

Looking at the list, we can see one large difference this time: the SMTP trigger now has two arrows, one pointing up and the other pointing down. Moving your mouse cursor over them, you will discover that they denote the same thing as before: the triggers that this particular trigger depends on or that depend on this trigger. If a trigger is in the middle of a dependency chain, two arrows will appear, as has happened for the SMTP service is down trigger here.

The arrows here are shown in the same direction as in our previous schematic. We could say that the dependent trigger is "supported" by the "more important" trigger, as if we had bricks placed one on top of another. If any of the bricks disappears, the bricks above it will be in trouble.

Our testfile trigger worked as expected for the chained dependencies, so we can remove that dependency now. Go to Configuration | Hosts, click on Triggers next to Another host, and click on the SMTP service is down trigger in the NAME column. Switch to the Dependencies tab, click on Remove in the ACTION column, and click on the Update button. Note that you always have to save your changes in the editing form of any entity. In this case, simply removing the dependency won't be enough. If we navigate to some other section without explicitly updating the trigger, any modifications will be lost. Now, you can also restart any stopped services on "Another host".

## Constructing trigger expressions

So far, we have used only very simple trigger expressions, comparing the last value to some constant. Fortunately, that's not all that trigger expressions can do. We will now try to create a slightly more complex trigger.

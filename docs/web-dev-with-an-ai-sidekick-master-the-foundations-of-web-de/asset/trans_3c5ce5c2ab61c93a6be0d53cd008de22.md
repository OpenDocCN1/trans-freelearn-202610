
```
import json
text = json.dumps(data, indent=2)
```

这将 Python 数据结构转换为格式化字符串。将该字符串写入磁盘属于正常的文件 I/O 操作。

读取 JSON 的过程与此相反，如下面的代码所示：

```
data = json.loads(text)
```

文件本身仍然是纯文本。JSON 只是赋予了该文本一种可预测的结构。

## 路径和文件位置

在小型脚本中，像 `tasks.json` 这样的文件名之所以有效，是因为 Python 会在当前工作目录中查找。在实际程序中，路径可能会变得更复杂。一个常用的处理路径的模块是 `pathlib`，如下面的代码所示：

```
from pathlib import Path
DATA_FILE = Path("tasks.json")
```

与手动操作字符串相比，Path 对象使文件操作更加清晰和安全，如下面的代码所示：

```
DATA_FILE.exists()
DATA_FILE.read_text(encoding="utf-8")
DATA_FILE.write_text(text, encoding="utf-8")
```

这些是建立在普通文件 I/O 之上的便捷方法。它们仍然遵循相同的规则，但减少了样板代码。

## 常见文件 I/O 错误

文件操作可能会因程序无法控制的原因而失败：

* 文件不存在。
* 文件被锁定或不可读。
* 磁盘已满。
* 内容格式不符合预期。

因此，文件 I/O 通常包裹在 `try` 和 `except` 块中，如下面的代码所示：

```
try:
    text = DATA_FILE.read_text(encoding="utf-8")
except OSError:
    print("Could not read file")
```

前面的代码防止程序崩溃，并让你决定如何恢复。

## 文件 I/O 总结

文件 I/O 赋予了编程项目持久性。没有它，任务会在程序退出时消失。有了它，你的程序可以在多次运行之间保持状态。

本节最重要的收获不是具体的函数，而是这种模式：

* 将内存中的数据转换为文本。
* 将该文本写入磁盘。
* 稍后将其读回。
* 将其转换回 Python 数据结构。

这种模式在 Python 开发中无处不在：配置文件、日志、缓存、数据库和 Web API。如果你在这个层面上理解了文件 I/O，以后关于保存和加载数据的任何内容都不会让你感到神秘。

**提示**：请展示一些常见的文件 I/O 错误以及如何避免数据丢失。

现在，让我们利用本章学到的所有知识构建一个完整的迷你项目。

# 一个小型、内聚的迷你项目

在本章的最后一节，我们将构建一个小型、内聚的迷你项目：一个命令行任务追踪器。

这个迷你项目将你迄今为止学到的所有内容串联起来：变量、条件语句、循环、数据结构、函数、模块/导入、字符串（包括 f-strings）、文件 I/O 和基本错误处理。

你将构建一个小型的命令行*任务追踪器*，它可以：

1. 添加任务
2. 列出任务
3. 将任务标记为完成
4. 删除任务
5. 从 JSON 文件保存和加载任务，使它们在运行之间持久化

你将保持数据模型简单：一个字典列表。每个任务将如下所示：

```
{"title": "Buy milk", "done": False}
```

## 编写代码

在 VS Code 中创建一个名为 `task_tracker.py` 的新文件并粘贴以下代码。先从上到下阅读一遍，然后运行它，再进行调整。

你可以从以下链接下载此完整文件：[https://github.com/markjprice/web-dev-ai/blob/main/Code/Chapter08/python-basics/task_tracker.py](https://github.com/markjprice/web-dev-ai/blob/main/Code/Chapter08/python-basics/task_tracker.py)

**良好实践**：如果你的代码使用类型提示，特别是涉及类或大型项目时，请在文件顶部添加 `from __future__ import annotations` 然后就不用再管它了。它在不增加复杂性的情况下消除了边缘情况。对于现代 Python（3.10+），如果你使用类型提示，通常建议在文件顶部包含此导入。它使注解更灵活，并避免了代码增长时出现的微妙问题。在更新的 Python 版本（3.13+）中，这种行为正成为默认行为，因此此导入最终将不再必要。

```python
from __future__ import annotations
import json
from pathlib import Path
DATA_FILE = Path("tasks.json")
def load_tasks() -> list[dict]:
    """Load tasks from disk. If the file doesn't exist yet, return an empty list."""
    if not DATA_FILE.exists():
        return []
    try:
        text = DATA_FILE.read_text(encoding="utf-8")
        data = json.loads(text)
        # Defensive check: ensure it's a list of dicts.
        if not isinstance(data, list):
            return []
        cleaned: list[dict] = []
        for item in data:
            if isinstance(item, dict) and "title" in item and "done" in item:
                cleaned.append({"title": str(item["title"]), "done": bool(item["done"])})
        return cleaned
    except (OSError, json.JSONDecodeError):
        # File can't be read or contains invalid JSON.
        return []
def save_tasks(tasks: list[dict]) -> None:
    """Save tasks to disk as JSON."""
    try:
        text = json.dumps(tasks, indent=2)
        DATA_FILE.write_text(text, encoding="utf-8")
    except OSError:
        print("Warning: could not save tasks to disk.")
def add_task(tasks: list[dict]) -> None:
    title = input("Task title: ").strip()
    if not title:
        print("Task title cannot be empty.")
        return
    tasks.append({"title": title, "done": False})
    save_tasks(tasks)
    print(f'Added: "{title}"')
def list_tasks(tasks: list[dict]) -> None:
    if not tasks:
        print("No tasks yet. Add one with: add")
        return
    for i, task in enumerate(tasks, start=1):
        status = "✓" if task["done"] else " "
        print(f"{i:>2}. [{status}] {task['title']}")
def mark_done(tasks: list[dict]) -> None:
    if not tasks:
        print("No tasks to mark as done.")
        return
    raw = input("Task number to mark done: ").strip()
    try:
        index = int(raw) - 1
    except ValueError:
        print("Please enter a valid number.")
        return
    if index < 0 or index >= len(tasks):
        print("That task number is out of range.")
        return
    tasks[index]["done"] = True
    save_tasks(tasks)
    print(f'Marked done: "{tasks[index]["title"]}"')
def delete_task(tasks: list[dict]) -> None:
    if not tasks:
        print("No tasks to delete.")
        return
    raw = input("Task number to delete: ").strip()
    try:
        index = int(raw) - 1
    except ValueError:
        print("Please enter a valid number.")
        return
    if index < 0 or index >= len(tasks):
        print("That task number is out of range.")
        return
    removed = tasks.pop(index)
    save_tasks(tasks)
    print(f'Deleted: "{removed["title"]}"')
def print_help() -> None:
    print("Commands:")
    print("  add     Add a new task")
    print("  list    List all tasks")
    print("  done    Mark a task as done")
    print("  delete  Delete a task")
    print("  help    Show this help")
    print("  quit    Exit the program")
def main() -> None:
    tasks = load_tasks()
    print("Task Tracker")
    print_help()
    while True:
        command = input("\n> ").strip().lower()
        if command == "add":
            add_task(tasks)
        elif command == "list":
            list_tasks(tasks)
        elif command == "done":
            mark_done(tasks)
        elif command == "delete":
            delete_task(tasks)
        elif command == "help":
            print_help()
        elif command in {"quit", "exit"}:
            print("Goodbye.")
            break
        elif command == "":
            continue
        else:
            print("Unknown command. Type: help")
if __name__ == "__main__":
    main()
```

## 尝试项目

运行代码文件并尝试这个快速会话：

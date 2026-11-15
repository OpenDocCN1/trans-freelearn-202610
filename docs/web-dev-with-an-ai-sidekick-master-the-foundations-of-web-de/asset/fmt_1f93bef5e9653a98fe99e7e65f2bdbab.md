
Articles: [https://github.com/markjprice/markjprice/blob/main/articles/README.md](https://links.uk.defend.egress.com/Warning?crId=6a06d4338e5d5f5540437bbb&Domain=packt.com&Threat=eNpzrShJLcpLzAEADmkDRA%3D%3D&Lang=en&Base64Url=eNpNxzEOgCAMBdAb8Xc3ExldvAGtRKpUSKn319HtveLexwQc4uWhwE2hya6zm3D-k2qj73IjmQvXPLDFeVlj0P0Fj_wZbg%3D%3D&@OriginalLink=github.com)

Now that you know what to expect from the rest of the chapters in this book, let's set up your development environment. Although we won't start building a full project until the *Survey Project* part of this book, during all the language-learning chapters you will want to write snippets of code and see them run for real.

# Setting up your web development environment

Before you start programming, you'll need a code editor for multiple languages including HTML, CSS, JavaScript, TypeScript, SQL, and Python, either from Microsoft or a third party. I recommend VS Code because it supports extensions for all common languages and runs cross-platform on Windows, Mac, and Linux.

VS Code is by far the most popular code editor or IDE across all developers with over 75% of professional developers selecting it in a Stack Overflow 2025 survey, which you can read at the following link: [https://survey.stackoverflow.co/2025/technology/#1-dev-id-es](https://survey.stackoverflow.co/2025/technology/#1-dev-id-es).

## Downloading and installing VS Code

Let's now download and install VS Code and some useful extensions:

1. Download and install either the Stable build or the Insiders edition of VS Code from the following link: [https://code.visualstudio.com/download](https://code.visualstudio.com/download), as shown in *Figure 1.3*:

If you need more help installing VS Code, you can read the official setup guide at the following link: [https://code.visualstudio.com/docs/setup/setup-overview](https://code.visualstudio.com/docs/setup/setup-overview).

![](Images/B34553_01_03.png)

Figure 1.3: VS Code download web page for Windows, Linux, and Mac

1. To install the VS Code extensions with a user interface, you must first launch the VS Code application.
2. In VS Code, click the **Extensions** icon or navigate to **View** | **Extensions**.
3. Enter `Live Preview` in the search box.
4. Select the **Live Preview** extension and confirm that it is the correct one, as shown in *Figure 1.4*:

    * **Identifier**: `ms-vscode.live-server`
    * **Version**: 0.4.16 or later
    * **Publisher**: Microsoft
    * **Downloads**: more than 11 million

![](Images/B34553_01_04.png)

Figure 1.4: Installing the Live Preview extension published by Microsoft

5. Click **Install** and wait for it to download and install.

VS Code has built-in support for editing HTML, CSS, JavaScript, and TypeScript, but other languages can be supported by installing more extensions.

## Installing other extensions

In later chapters of this book, you will use more VS Code extensions. If you want to install them now, all the extensions that we will use are shown in *Table 1.1*:

| **Extension name and identifier** | **Description** |
| --- | --- |
| **Live Preview** `ms-vscode.live-server` | Hosts a local server in your workspace for you to preview your web pages. |
| **Markdown All in One** `yzhang.markdown-all-in-one` | All you need for Markdown (keyboard shortcuts, table of contents, auto preview, and more). |
| **SQLite** `alexcvzz.vscode-sqlite` | Explore and query SQLite databases. |
| **Python** `ms-python.python` | Rich support for the Python language (for all actively supported Python versions), providing access points for extensions to work with and offer support for IntelliSense (Pylance), debugging (Python Debugger), formatting, linting, code navigation, refactoring, and so on. |
| **Django** `batisteo.vscode-django` | Beautiful syntax and scoped snippets for perfectionists with deadlines. |
| **GitHub Copilot** `github.copilot` | AI peer programming tool that helps you write code faster and smarter. |
| **Claude Code for VS Code** `anthropic.claude-code` | Claude autonomously explores your codebase, reads and writes code, and runs Terminal commands with your permission. |
| **Gemini Code Assist** `google.geminicodeassist` | AI-assisted development powered by Gemini. |
| **Playwright Test for VSCode** `ms-playwright.playwright` | Run Playwright Test tests in VS Code. |

Table 1.1: VS Code extensions for web development

You can install a VS Code extension at the command prompt or terminal, as shown in *Table 1.2*:

| **Command** | **Description** |
| --- | --- |
| `code --list-extensions` | List installed extensions |
| `code --install-extension <extension-id>` | Install the specified extension |
| `code --uninstall-extension <extension-id>` | Uninstall the specified extension |

Table 1.2: Managing VS Code extensions at the command prompt

For example, to install the **Live Preview** extension, enter the following in the command prompt:

```
code --install-extension ms-vscode.live-server
```

I have created PowerShell scripts to install and uninstall the VS Code extensions in the preceding table. You can find them at the following link: [https://github.com/markjprice/web-dev-ai/tree/main/scripts/extension-scripts/](https://github.com/markjprice/web-dev-ai/tree/main/scripts/extension-scripts/). PowerShell scripts are cross-platform, as you can read about at the following link: [https://learn.microsoft.com/en-us/powershell/scripting/overview](https://learn.microsoft.com/en-us/powershell/scripting/overview).

## Understanding VS Code versions

Microsoft releases a new feature version of VS Code (almost) every month and bug-fix versions more frequently. For example:

* Version 1.109.0, January 2026 feature release
* Version 1.109.1, January 2026 bug-fix release

The version used in this book is 1.112.0, the April 2026 feature release, but the version of VS Code is less important than the versions of the extensions that you install. Make sure to keep all your extensions up to date.

## Keyboard shortcuts for VS Code

If you want to customize your keyboard shortcuts for VS Code, then you can, as shown at the following link: [https://code.visualstudio.com/docs/getstarted/keybindings](https://code.visualstudio.com/docs/getstarted/keybindings).

I recommend that you download a PDF of VS Code keyboard shortcuts for your operating system from the following list:

* Windows: [https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)
* macOS: [https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
* Linux: [https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)

Now that you have set up your development environment, let's dive into how to get the best out of an AI chatbot when you want it to act as your coach for learning web development.

# Learning with an AI chatbot as your coach

AI chatbots like ChatGPT can be useful learning tools when you use them well.

This section compares three chatbots that are commonly used for learning web development.

**Good practice**: If you are new to web development or unsure which tools to choose, use the following setup. This is the simplest path through the book, and what the examples assume:

* **Chatbot**: ChatGPT (free or Plus at $20 USD per month)

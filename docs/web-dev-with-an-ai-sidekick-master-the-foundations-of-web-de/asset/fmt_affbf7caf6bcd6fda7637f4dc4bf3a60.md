
# Frontend Web Development

## [Chapter 2: HTML – Structuring Your Web Pages](#02.xhtml#_idTextAnchor098)

Covers the basics of HTML (HyperText Markup Language). You’ll use the AI to generate a simple page and, in doing so, learn about elements, tags, and attributes. You’ll learn about HTML’s role in defining web content structure (from headings to links and images). You will create a contact form, introducing form elements.

## [Chapter 3: CSS – Styling Your Web Pages](#03.xhtml#_idTextAnchor154)

Shows you how to control presentation and layout. You will get the AI to explain selectors, properties, and values. You will apply styles to your web page from [Chapter 2: HTML – Structuring Your Web Pages](#02.xhtml#_idTextAnchor098). You will learn about responsive design and making mobile-friendly web pages using CSS media queries.

## [Chapter 4: SVG – Drawing Scalable Graphics](#04.xhtml#_idTextAnchor212)

Introduces SVG as a way to generate graphics with code, which will be useful for charting survey results. AI will explain that SVG is an XML-based markup language for 2D graphics and how it differs from canvas/bitmaps. Through AI-guided examples, you create basic shapes (circle, rectangle, line) in SVG and embed them in HTML.

## [Chapter 5: JavaScript – Making Web Pages Interactive](#05.xhtml#_idTextAnchor266)

Covers fundamental JavaScript concepts like variables, functions, events, and the DOM. We start by describing what JavaScript is (a lightweight, interpreted language that runs in browsers) and where to include `<script>` blocks in HTML.

## [Chapter 6: TypeScript – Safer JavaScript](#06.xhtml#_idTextAnchor316)

Introduces TypeScript as a superset of JavaScript that adds static types and explains why they are useful.

# Backend Web Development

## [Chapter 7: SQL – Managing Data](#07.xhtml#_idTextAnchor374)

Demystifies SQL (using SQLite and a simple database for practice). The AI explains what a database is and how SQL is used to query data. You will practice basic SQL commands through AI prompts: `SELECT` statements, filtering (`WHERE`), and sorting (`ORDER BY`). The AI can generate a sample dataset (like a `users` table) and challenge you with queries (“How would you get all users who signed up this month?”).

## [Chapter 8: Python – Programming on the Backend](#08.xhtml#_idTextAnchor494)

Covers fundamental Python syntax and concepts, preparing readers for backend web development. You’ll install Python and then learn coding for Python basics like variables, loops, and functions. You will ask the AI to explain code line by line, reinforcing understanding.

## [Chapter 9: Django – Web Development on the Backend](#09.xhtml#_idTextAnchor598)

Focuses on Django, Python’s most commonly-used web framework. Your AI will explain how Django works with its `MVT` (Model-View-Template) pattern, which is related to but distinct from the traditional `MVC` (Model-View-Controller) pattern, and why it’s used for rapid web development. With AI guidance, you start a Django project (the book provides step-by-step instructions, and the AI can help troubleshoot). You’ll create a new Django project, which lays the groundwork for the full project in the *Survey Project* part of the book. The AI helps you write a simple view and a template. You learn how Django serves HTML and how to use template variables.

## [Chapter 10: Bash/Shell – Command-Line Skills for Developers](#10.xhtml#_idTextAnchor689)

Briefly steps aside from web programming to teach Bash/Shell usage, since many development tasks involve the command line, especially to automate deployments. You will practice common commands (`ls`, `cd`, `mkdir`, `rm`, etc.) by having the AI describe what each does and trying them in a terminal or emulator. The AI will walk you through creating a small Bash/Shell script to automate a task like backing up a file or starting your Django server.

# Survey Project

## [Chapter 11: Setting Up Your Project Environment](#11.xhtml#_idTextAnchor764)

Kicks off the TallyApp survey project and configures tools. You will see an overview of the survey app you’ll build and its features, like creating surveys, taking surveys, and viewing results. Step-by-step instructions (with screenshots or code snippets) for setting up `VS Code` with `GitHub Copilot` are provided. You will be introduced to a `Docker`-based development environment and how to place code under version control using `Git`.

## [Chapter 12: Unit, Integration, and End-to-End Testing](#12.xhtml#_idTextAnchor809)

Teaches you how to design and implement a layered testing strategy for TallyApp, write meaningful unit and integration tests in Django, automate real browser interactions with end-to-end tests using `Playwright`, and use AI tools to generate tests automatically.

## [Chapter 13: Specifying and Building the Initial Project](#13.xhtml#_idTextAnchor872)

Defines a clear project brief for TallyApp, translates that into an actionable development plan, guides you to scaffold a `Django 6` project with appropriate apps and models, and implements core authentication and survey functionality.

## [Chapter 14: Adding Project Features Using Spec-Driven Development](#14.xhtml#_idTextAnchor922)

Covers how to implement a structured feature specification, use an AI coding assistant to implement it inside an existing Django project, and verify that the implementation satisfies clearly defined acceptance criteria.

## [Chapter 15: Deploying to Public Servers](#15.xhtml#_idTextAnchor977)

Reviews options for deploying TallyApp to a server connected to the wider internet so that members of the public can use the website. You will deploy TallyApp to a publicly accessible server, understand how requests flow through a production Django stack, know how to preserve and back up user data across deployments, and know what has to change as traffic and expectations increase.

## Epilogue

Reviews the next steps on your web development learning journey.

## Appendix

Contains answers to questions at the end of each chapter.

# Why These Languages and Skills?

You might wonder why I’ve chosen to cover these specific languages, frameworks, and skills. One major reason is that `Stack Overflow` conducts a survey of thousands of developers every year and one question they ask is: *Which programming, scripting, and markup languages have you done extensive development work in over the past year?*

The 31,771 responses to this question in the 2025 survey revealed the top six languages that developers reported using extensively over the past year are JavaScript, HTML/CSS, SQL, Python, Bash/Shell, and TypeScript, as shown in *Figure 1.1*:

![](Images/B34553_01_01.png)

*Figure 1.1: 2025 survey of the most extensively used programming languages*

You can see these results in the survey at the following link: [https://survey.stackoverflow.co/2025/technology#most-popular-technologies-language](https://survey.stackoverflow.co/2025/technology#most-popular-technologies-language)

These results make sense when you realize that all those languages are needed to do web development, and most programming work these days is web development. All that’s missing in the top six are SVG for drawing scalable vector graphics, and Django which is the most popular web development framework for Python. I even briefly considered naming the book, *Top Six Programming Languages*, but ultimately decided that web development is a better connecting theme for a book that covers all those languages.

You can see a summary of what roles each of the languages and frameworks play in web development in *Figure 1.2*:

![](Images/B34553_01_02.png)


There is not one standard way to do spec-driven work. Different teams structure their specifications differently, use different terminology, and automate different parts of the process. Some developers prefer lightweight Markdown documents stored alongside the source code. Others use structured templates that resemble traditional requirements documents. Some processes emphasize automated tests as the executable form of the specification, while others focus on planning artifacts that guide AI tools through staged implementation.

A small ecosystem of tools has begun to appear to support this style of development. One example is **Spec Kit**, an open-source project from GitHub that provides templates and commands for writing specifications, generating implementation plans, and breaking features into tasks. Another tool is **OpenSpec**, which aims to standardize machine-readable software specifications that can be interpreted by AI agents. Some developers are also experimenting with specification-first workflows built into AI coding environments such as Cursor, Continue, and Claude Code, where structured prompts and planning steps guide the model through feature development.

**Warning!** Because GitHub Spec Kit, OpenAI Codex, Anthropic Claude Code, and other AI tools are evolving quickly, treat exact prompts and small setup details as likely to drift over time.

Now that you've been introduced to some spec-driven development tools, let's look in more detail at one practical option.

# Using Spec Kit to structure specifications

One practical tool that supports Spec-Driven Development is **Spec Kit**, an open-source project published by GitHub. It provides a structured workflow and templates for writing specifications, planning implementations, and coordinating AI-assisted development.

Spec Kit is not a framework or library. Instead, it is a lightweight system that helps developers organize their specifications so that both humans and AI assistants can understand them clearly.

## The Spec Kit workflow

Spec Kit organizes development into a sequence of stages. Each stage produces a document that clarifies the system before moving on to the next step.

A typical workflow looks like this:

### 1. Project principles, also called the constitution

A document that defines the high-level rules and philosophy of the project. These might include conventions such as coding standards, architectural constraints, or testing requirements.

Example questions addressed in this stage:

* Should the system prioritize simplicity or extensibility?
* What testing practices are required?
* What technologies are allowed or forbidden?

This document sets the rules the AI should follow when it changes the project.

### 2. Feature specification

The specification describes the feature that will be built.

It typically includes:

* The goal of the feature
* Non-goals and constraints
* User stories
* Acceptance criteria
* Data model changes
* Error handling and edge cases

This is the stage where you define exactly what the feature must do.

### 3. Implementation plan

Once the specification is approved, the next document describes how the feature will be implemented.

The plan usually includes:

* Architecture changes
* New modules or services
* Database migrations
* Testing strategy
* Deployment considerations

AI tools are particularly useful in this stage because they can suggest implementation approaches based on the specification.

### 4. Task breakdown

The implementation plan is then converted into a list of concrete development tasks.

For example:

* Create ShortLink Django model
* Add redirect view
* Create form for generating short links
* Write unit tests

These tasks can be executed sequentially, often with AI assistance.

### 5. Implementation

Only after the specification, plan, and tasks are defined does the actual coding begin.

At this stage, the AI assistant is given clear instructions and constraints. Instead of inventing behavior, it simply implements what has already been described.

**Prompt**: Please explain how tools like Spec Kit and OpenSpec help prevent AI coding assistants from introducing unintended features.

Structure helps, but it has a cost. Before using Spec Kit for every change, it is worth knowing when a lighter approach is enough.

## When is Spec Kit overkill?

Spec Kit can help keep AI-generated work on track, but it can be too much for small changes. You are trading some coding time for more planning and review time.

Let's understand when Spec Kit is likely overkill or unnecessary.

### When a lighter prompt is enough

If you are in the exploratory phase, for example, prototyping a UI or testing a basic concept, Spec Kit's rigid flow (`/constitution` → `/specify` → `/plan` → `/tasks`) can slow you down.

Small projects often benefit from vibe coding, where you give a high-level prompt and iterate on the code directly. Setting up a Constitution for a weekend hobby project is overkill.

### High context-to-code ratio

Spec Kit can generate hundreds of lines of Markdown for a single feature. For a small project (like a basic URL shortener with two views), the documentation can be longer than the code. The exception is when learning because a smaller project is easier to understand.

The risk is that you spend more time reading and correcting the AI's plans and specs than it would take to simply write the code or review a single-shot AI output.

### Minor tweaks and one-off bug fixes

If you're just changing a button color, updating a regex, or fixing a simple logic error, just prompt your AI coding tool directly. Running a full `/specify` and `/plan` cycle for a 5-line change is inefficient and adds unnecessary files to your repository.

### Token cost and speed

Each step in the Spec Kit workflow involves significant back-and-forth with the LLM, consuming thousands of tokens and taking several minutes of processing time.

For a small project, you want the result in 10 seconds. Spec Kit makes you wait for the AI to think through the architecture, which is often unnecessary for well-trodden paths (like a basic Django app).

## Birgitta Böckeler's taxonomy

Spec-Driven Development is best understood as a spectrum. At one end, you might write a short prompt that captures intent. At the other, you might produce a full specification with acceptance criteria and validation steps. *Birgitta Böckeler* describes spec-driven work as a spectrum rather than a single rigid process.

* Birgitta Böckeler's * taxonomy of Spec-Driven Development is not a formal standard like a published specification. It is a practical way of describing how it shows up at different levels of rigor when working with AI.

### Lightweight

You still think before you code, but the spec is minimal. It might be a short paragraph, a checklist, or even a well-crafted prompt. Typical characteristics:

* A brief description of the goal
* Maybe a couple of constraints or edge cases
* Often embedded directly in the AI prompt
* Little or no separate artifact

Use this for small features, bug fixes, and exploratory work where you are iterating quickly. The key idea is that you are guiding the AI, but not formalizing everything. This still counts as SDD because you describe the change before coding.

### Structured

This is closer to what we will see in this chapter. Typical characteristics:

* A clearly written specification
* Defined behavior and user interactions
* Data model considerations
* Acceptance criteria or test cases
* Separate from the implementation step

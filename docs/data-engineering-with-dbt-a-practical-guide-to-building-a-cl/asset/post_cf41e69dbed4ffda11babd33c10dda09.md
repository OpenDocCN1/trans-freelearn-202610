
Another important concept is the active management of technical debt. While it is fine and normal that some parts of the code base might get released even if they’re not up to standard, this must be for a real contingent reason and the team must repay that debt, fixing the lower-quality code once the contingent reason has passed.

In the following subsections, we discuss some of the software engineering best practices that can help data teams to work in a more satisfying way.

Note that they are very much aligned with the ones that Tristan Handy, the founder of dbt-labs, laid out in a seminal post when starting work on dbt. You can read the 2016 post from Tristan at this URL: https://blog.getdbt.com/building-a-mature-analytics-workflow/.

## Version control

Having a central repository with all the code stored and organized for everyone to access, with a clear understanding of the succession of the versions, is already an improvement over many situations where SQL files were saved to shared disks and versions maintained by file or folder name.

The ability to create branches to work independently and then merge the changes simply enables the parallel development of features and provides a safe way to experiment with changes.

Anyway, adopting code version control is not only useful by itself but is also the required step to enable many positive processes.

The version control system, such as Git, becomes the integration hub that can trigger collaboration and automation processes such as the following ones:

- **Pull requests:** This powerful feature allows the team to look at the changes proposed by a developer and comment on them, asking for clarifications and proposing improvements, all the while getting up to speed with the latest changes.
- **Automation:** When new code is checked in, many kinds of automation can be triggered, from code quality tools that analyze the new code to automated tests, all the way to continuous integration and continuous deployment.
- **Authorization:** Especially in very bureaucratic or heavily regulated environments, the version control system can work as a recording tool for approvals, making that required step trivial.

## Quality assurance

The three cornerstones of DataOps, ownership, freedom, and responsibility, heavily rely on the fact that the team has a solid quality assurance process in place.

The best way to be sure that the code that you have in production works as expected and that the data that comes in is as you expect is to have automated tests that are executed every time that new code is released or that new data is loaded.

The modern data stack allows you to build reusable models so that you can code the customer only once, maybe composing it by putting together lower-level models that represent the online customers, the store customer and the wholesale customer. All of them are useful refined models that can be reused in multiple contexts.

The key here is that technology and business must align to define the relevant business concepts, and the technology then helps to implement them, reducing code duplication and promoting the use of stable interfaces.

We will look more into how to write modular code in the *Designing for maintainability* section.

## Development environments

To provide your development team with the ability to quickly iterate over code and develop and test new functionalities, every developer must have their own environment where they can work without the need to wait or synchronize with others and without the fear of breaking something.

Being free to run your code when you are ready, to run tests that give you trusted answers in seconds or minutes, or being able to break and recreate your environment in the same short time are essential abilities that the modern data stack provides. These enable developers to focus on writing good, clean business code and not on support or side tasks, let alone just waiting.

On the contrary, when developers must try out their code in production, they cannot afford to make errors because they cannot fail without causing relevant issues. The result is fear of changing any piece of code and releases taking forever, as every little change has to be thoroughly considered and weighed against consequences.

Even in the case where there is only one shared development environment or a few that are slow or expensive to create and therefore must be shared and well maintained… the result is similar: fear of change and demotivation because of time lost waiting for an environment or from misleading results because the dev environment does not reflect the real environment.

After all, this should not be very surprising as most people do not like waiting in a queue, and are even less keen on taking a walk on a cable suspended high up, with or without a safety net.

Personal development environments are a key enabler for the ability to quickly develop and test new functionalities. How can you release daily or even multiple times a day if each developer waits half a day to try out their code?

Continuous integration is the next step in increasing the speed and trust in the development process, making sure that all the individual features that have been developed are working well together. It provides another, stronger level of confidence, reinforcing the awareness that all the code being released has been reviewed and discussed by the team.

Continuous deployment provides the last step, allowing the code tested in the integration environment to be released to production. That can be done automatically or upon manual code promotion.

## Designing for maintainability

We have kept for last the most important of all the best practices: develop being mindful about the future, that is, taking into consideration evolution and maintenance.

Data platforms are some of the longest-living enterprise applications, so while you are writing one, you can be assured it will be in use for a long time. This means that over time, there will be a lot of changes in data sources as well as business rules, and even more, questions will arise about the data managed in the data platform.

That in turn means that a lot of people, including yourself in the next months and years, are bound to come back and reread that code. This makes the case for writing code that is understandable for humans.

Writing code that just works is not enough, just like putting a control half-hidden in a place that is difficult to access is not a good design. Design your code so that the next person to touch it will thank you for putting the control where it is easy to see and reach.

Writing maintainable code means writing code that is easy to understand and change for your fellow human colleagues. Avoid long and complex queries by breaking down your logical steps into individual **Common Table Expressions (CTEs)** so that you can provide proper naming for each step and create clean, readable code. Keep your models under 100 lines and the number of joins to two or three, unless a model has the single purpose of joining many tables.

In this area, the evergreen book *Clean Code* by Bob Martin and the articles by Martin Fowler on architecture, refactoring, and agility will be great guides; even if the code samples and the main discussion may be about Java and application software, the ideas apply to every programming context.

Performance and correctness are a consequence of well-architected code that is also clearly expressed; they are never an excuse to write obscure code or overly complicated logic.

Remember, the less code there is, the fewer bugs will be in it.

The fact that we might be using SQL or some Python-based scripting to express our logic does not mean that we should settle for anything less than good, readable code and clearly expressed business logic. After all, if the code is easy to understand, it is also easy to verify its logic by humans and automatically test that it does what is expected, while that is not the case if the code is obscure or the business logic is not clearly understood.

The other key part of writing maintainable code is making sure that your code does not have to change every other day and that when you need to change it, it is easy to change. For that, we can resort to the whole body of knowledge of software engineering, with special regard to the SOLID principles from object-oriented programming. The following are some principles that are easy to put into use by anyone in data projects:

- **SRP**: The **single-responsibility principle** says that a model should have one, and only one, reason to change. In practice, this means that a model should focus on doing one thing, hopefully doing it well and making obvious what it does. This means the opposite of the huge query that does everything at once and more if possible.

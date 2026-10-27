Another important concept is the active management of technical debt. While it is fine and normal that some parts of the code base might get released even if they’re not up to standard, this must be for a real contingent reason and the team must repay that debt, fixing the lower-quality code once the contingent reason has passed.

In the following subsections, we discuss some of the software engineering best practices that can help data teams to work in a more satisfying way.

Note that they are very much aligned with the ones that Tristan Handy, the founder of dbt-labs, laid out in a seminal post when starting work on dbt. You can read the 2016 post from Tristan at this URL: https://blog.getdbt.com/building-a-mature-analytics-workflow/.

# Version control

Having a central repository with all the code stored and organized for everyone to access, with a clear understanding of the succession of the versions, is already an improvement over many situations where SQL files were saved to shared disks and versions maintained by file or folder name.

The ability to create branches to work independently and then merge the changes simply enables the parallel development of features and provides a safe way to experiment with changes.

Anyway, adopting code version control is not only useful by itself but is also the required step to enable many positive processes.

The version control system, such as Git, becomes the integration hub that can trigger collaboration and automation processes such as the following ones:

+   - **Pull requests:** This powerful feature allows the team to look at the changes proposed by a developer and comment on them, asking for clarifications and proposing improvements, all the while getting up to speed with the latest changes
+   - **Automation:** When new code is checked in, many kinds of automation can be triggered, from code quality tools that analyze the new code to automated tests, all the way to continuous integration and continuous deployment
+   - **Authorization:** Especially in very bureaucratic or heavily regulated environments, the version control system can work as a recording tool for approvals, making that required step trivial

# Quality assurance

The three cornerstones of DataOps, ownership, freedom, and responsibility, heavily rely on the fact that the team has a solid quality assurance process in place.

The best way to be sure that the code that you have in production works as expected and that the data that comes in is as you expect is to have automated tests that are executed every time that new code is released or that new data is loaded.
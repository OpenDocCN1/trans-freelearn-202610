
# Managing the backlog in an agile way

What then should be in the **backlog**, or make up the list of work to be done?

The backlog must contain the description of the work to be done, but it does not mean that everything should be at the same level of detail or that the work to be done will not change.

The golden rule is that it should describe the work to be done in the next iteration with the most detail, containing enough information for the developers to fully understand what to do. It can also contain the description of other work to be done in the future in decreasing detail, as it is expected to be done further away in time. Future work should be in few and progressively more general stories.

At every iteration, aka sprint, you will get rid of the many small stories that you have developed, and you will split down into small, detailed stories and tasks the bigger stories that you pick from the backlog and plan to work on next.

> **Tip**
> 
> There is a common conflict about what to put in the backlog. For the development team, it is better to have just enough stories to fill a bit more than the current iteration, and any more is a burden, while the product owner is interested in keeping track of features to be developed further down the line and longer-term plans and ideas.

If reading through all the stories in your backlog takes too much, it is an indication that you have too many stories in your backlog. We strongly suggest grouping the ones that are far away into bigger, general stories. If the scope of the project is large, it makes sense to keep separated the backlog of the product owner, which is more a list of ideas and features considered for development, from the backlog of the development team, which should contain only the features that need to be developed in the next few iterations, as things further away are uncertain and likely to change.

This is just a basic principle of work environment hygiene – keep your backlog clean as you keep your desks and your offices. Nobody works well in offices full of dusty boxes or on tables overflowing with long-forgotten piles of paper. Why would you keep your backlog in such a shape?

As an example, you know that to develop report XYZ, you will need to load 10 tables and calculate a few more intermediate tables and so on. This is valuable information, but if you do not plan to do that work in the next sprint or so, it is much better to have that information as a piece of text and bullet points inside a single feature than having many individual stories. Until it is almost time for its development, it is easier to manage a single item with the `develop report xyz` title and then split it down into individual stories when you are about to work on it, splitting it into tasks that can be completed in less than 1 day once you put them into a sprint. This general story should be moved to the development team backlog when it is almost time to develop it, while it can stay in the product owner backlog when it is just one potential feature to be developed somewhere in the future.

Putting it in a more practical way, using a common terminology to identify work to be done at different levels of granularity, the backlog should contain the following:

- One or two sprints’ worth of **stories** that are ready to be developed. These stories must be described at an implementation detail level so that everyone in the team understands what needs to be done. The stories taken into development must be split into **tasks** before starting work on them. Initially, the tasks can be a bullet list of to-dos in the story description. When you split them, you should make sure that each task can be done in less than a day.
- A handful of **features** that describe the work that is planned and potentially ready to be done but is still being analyzed in detail and split into individual stories. These should provide work for another couple of sprints or so, and no more.
- Eventually, a few very general **epics** that list the next areas of development that are not yet planned or planned further away than a handful of sprints. These are a few, high-level descriptions of future work, mostly of interest to the product owner. It is perfectly fine if they contain text or documents, such as presentations with the expected content as it was understood at the time of their preparation. Review them critically before starting the work based on old plans.

This seems so simple to do, but I can assure you that in the vast majority of projects, the backlog hygiene is dissatisfying at best, with a lot of festering stories that will never be done, left there to consume everybody’s attention for nothing.

Sifting through hundreds of stories to find the handful of ones to be done in the next sprint or to update with your findings is more complicated, takes longer, and reduces focus than doing the same with a dozen or two stories.

What is the value of a “make report X” or “load table Y” story with no extra content? None! It just clutters your backlog and kills your productivity when planning work or using the backlog.

Ask yourself why it should be worth keeping stories if they are untouched for months. Delete them and keep your backlog short and focused. You will remember those stories if you need them later, and if you do not remember them, it means that they were not important. If there is truly valuable information in an old story, you can save it into a higher-level story, but if all that you have is a title, just get rid of it.

## An initial backlog for our sample project

In the rest of this section, we will give you a practical example of managing the backlog, along with a tried and tested way of doing one of the most common tasks in data engineering – building reports.

We will describe our architecture following the way that data flows, so we have sources, then the storage layer with STG and SNAPSHOT or HIST models, then the refined layer with many specialized models, and finally, the delivery layer, where you have one data mart for each domain that powers the reports for the domain.

![](img/8868d272a75eee8b407a8e0f3434d10f_359_0.png)

Figure 10.2: Pragmatic Data Platform architecture and iterative analysis and development

When building reports, as often in agile methods, we start from the end of the flow to identify the goal and work our way back to identify the minimum work required to achieve the goal.

The previous diagram visually shows the main direction of the analysis and development work, with the dotted loopback circles recalling that, in both cases, these are iterative processes, and whenever we find a blocker or a problem, we can go back and change the results of the previous steps.

As an iterative process, we mean that in each iteration or set of iterations, we try to analyze and develop a small horizontal slice of our scope, covering all layers, to then move to another slice.

Thinking about the sample project that we have used in previous chapters, we could say that the initial overview produced the following epics:

- E1 – setting up the platform
- E2 – building customer reports
- E3 – building backlog reports

The first epic, E1, would cover the work that we did in the first few chapters to set up the platform. It would have been sliced into features such as “set up a Snowflake account,” “set up a dbt account,” and “set up a GitHub account.”

The Snowflake feature could have been divided into stories such as “create a Snowflake account,” “create a database for the dbt project,” “create roles for the dbt project,” “create a user for dbt to execute,” “create users for developers,” and so on. Each of these stories would be split into tasks if we thought that it could take more than 1 day for them to be completed.

There’s no need to split a story if it is already small enough. Remember that agility is also about avoiding useless work.

The second epic (E2) might be sliced into features such as the following:

- F1 – building a customer current position report
- F2 – building a customer historical position report
- F3 – building a customer transaction report
- F4 – other customer reports

We have willingly named the F4 feature “other customer reports” to show that it makes more sense to have a single feature with a potentially long list of desired reports inside, rather than producing a long list by already creating one feature for each report when, now, we would be many sprints away before they would be considered to be worked on.

### Tip

> Keep the backlog short by putting faraway work at all levels in an “all the rest” item or in a separate product owner backlog. The meaning of “faraway” depends on sprint duration and the amount of work completed during a sprint.

A good rule of thumb is to have stories ready for a sprint or two and identified features split into stories to provide work for a handful of sprints. Everything else should be in the product owner’s backlog or with the “all the rest” type items.

## Building reports in an agile way

In this section, we will decompose in stories a feature to build a report in a very general way that you can use for any report and, pretty much, for any other result deliverable in a data mart.

The “F1 – building a customer current position report” feature can be split into stories of these five types:

- S1 – designing a light data model for the data mart to power the F1 report.
- S2 – designing a light data model for the REF layer to power the data mart for the F1 report.
- S3.x – developing with dbt models the pipeline for the DIM_x / FACT_x / REF_x table. This is not one story but a story for each table in the data mart, plus each necessary support table in the REF layer. We will discuss more details later.
- S4 – an acceptance test of the data produced in the data mart.
- S5 – development and verification of the report in the BI application.

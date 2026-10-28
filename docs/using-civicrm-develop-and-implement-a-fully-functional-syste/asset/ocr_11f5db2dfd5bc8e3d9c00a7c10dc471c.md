After you have finished adding all activity types that apply to this case type, be sure to add a closing </ActivityTypes> tag in your XML file.

The next configuration block is where you define your timeline(s). It is labeled ActivitySets in the configuration file. Before we construct it, let's review exactly what a timeline is and what it does.

The purpose of a timeline is to establish a series of activities that happen in a defined order and are targeted for a period of time relative to other activities (dependencies). In addition:

+   - Timelines are optional. If your case type is primarily used for grouping activities and not to establish a defined workflow, you may not need to define a timeline beyond the initial Open Case activity.
+   - Timelines may be implemented automatically when a case is opened or made available as an option that can be applied to an already open case. In the first scenario, the activities defined in the timeline are automatically added to the case when it is opened. In the latter, the option of adding the timeline is available within the Manage Case screen.
+   - More than one timeline can be defined for a single case type. With the ability to make timelines optional and manually applied, this can allow for fairly complex workflow branching. For example, in our Grant Application use case, we could construct an initial timeline that takes us through the application process, and then create manually-applied timelines that address the workflow requirements based on our decision tree, whether the application is accepted, declined, or more information is required.
+   - Timelines can be used for audit reporting. The concept of an audit report is to track the progress of case resolution against a defined standard. For example, if you've defined a certain step in the process that needs to be completed within five days, you may want to run analysis on actual completion dates to determine if that window is too short (case workers are chronically late completing the task) and make adjustments to the workflow.

Let's look at the construction of our XML configuration file and the options available to better understand how timelines work.

The timelines will be grouped together inside the <ActivitySets> tag and each timeline will reside inside an <ActivitySet> tag (note the use of plurals and singulars to define the containing group and individual sets). Each set consists of the following tags:

+   - <name>: Unique name for the timeline. This should not contain spaces or other special characters. For example, you can name your initial timeline as standard_timeline.
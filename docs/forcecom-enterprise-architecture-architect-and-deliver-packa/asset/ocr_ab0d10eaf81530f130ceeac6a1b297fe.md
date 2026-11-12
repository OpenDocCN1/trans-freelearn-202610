# # 13 Source Control and Continuous Integration

So far we have been making changes in the packaging org for the FormulaForce application. This has worked well enough, as you're the sole developer in this case. However, when you add more developers and teams other considerations comes into play, mainly traceability of code changes and also the practical implications of conflicts when more than one developer is working in a development org at a time.

This chapter sees the packaging org take on more of a warehouse-only role, accessed only when there is a need to create a beta or release package and no longer for day-to-day development as we have been doing. This principle can become quite important to you and your customers if they desire that you demonstrate compliance and auditability with respect to controls around your software development process and life cycle.

As the code base grows and the number of contributions to it at any given time increases, it's important to monitor the quality of the code more frequently. Finding out that your Apex tests are failing prior to an upload is frustrating and costly when you're trying to release the software to your customers. Continuous Integration works with a Source Control system to validate the integrity and quality (through your tests) of your application on an automated basis, often triggered by developers committing changes to Source Control.

In this chapter, you will learn the following:

+   • Developer workflow
+   • Developing with Source Control
+   • Hooking up to Continuous Integration
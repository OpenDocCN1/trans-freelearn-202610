
## Creating a transformation with Pentaho reports

Now that you know what a Pentaho transformation is, and the basics around its development, let's see a concrete example, defining an easy ETL generating the PDF output of the well known `my_first_report.prpt` file. The first task to do is to copy the `my_first_report.prpt` file into the `data-integration` folder of your Pentaho Data Integration installation. This is required because it will be used as the current path during the execution of the transformation. If you don't have the `my_first_report.prpt` file available, you can download it from the https://github.com/fcorti/pentaho-8-reporting-for-java-developers repository, specifically, from the `Chapter 15 - Using reports in Pentaho Business Analytics Platform/my_reports` folder.

As a first step, let's run the Spoon GUI to start the development: open a Terminal, move to the `data-integration` folder, and run the `spoon` script. From the upper menu, click on the File | New | Transformation item to create an empty transformation. Once done, right-click on the empty right panel and select Properties. In the opening window, replace the `my first transformation` value with the content of the `Transformation name` field. Then click the OK button and press the disk icon (💾) on the upper toolbar. Ensure that you save the `my first transformation` file in the `data-integration` folder.

Now that the transformation is saved as empty, select the Design tab from the left panel and you will find all the available steps to include. Expand the `Input` set, and drag the `Data Grid` step and drop it into the right panel. Double-click on the `Data Grid` step and fill the Meta tab of the edit window, as shown in the following screenshot:

![](img/9337ac94537bf7e092e1ea6531dce8fd_681_0.png)

Before leaving the step configuration, select the Data tab and fill it as shown in the following screenshot:

![](img/9337ac94537bf7e092e1ea6531dce8fd_681_1.png)

With this action, the first step of the transformation is defined, generating a simple data stream with only one row containing two fields: the prpt_path field with value ./my_first_report.prpt, and the output_path field with value ./my_first_report.pdf. If you want to see how a data stream looks, click on the Preview button. Once done, click the OK button to confirm the Data Grid setup.

Back to the Design tab in the left panel, expand the Output set, and drag the Pentaho Reporting Output step and drop it into the right panel, on the right of the Data Grid step. To instruct the transformation to execute the two steps in the right sequence, click on the Data Grid step, and drag on the right connection icon and drop it to the Pentaho Reporting Output step. The following screenshot shows a visual description of the task:

![](img/9337ac94537bf7e092e1ea6531dce8fd_682_0.png)

The final result should look like the following:

![](img/9337ac94537bf7e092e1ea6531dce8fd_682_1.png)

Now that the data flow is set up, double-click on the Pentaho Reporting Output step and fill in the options as shown in the following screenshot:

![](img/9337ac94537bf7e092e1ea6531dce8fd_682_2.png)

Click on the OK button to confirm the step configuration and then click on the save (💾) icon in the toolbar to save the updated version of the transformation. Now that the transformation is developed, run it by clicking on the play icon (▶) in the transformation panel; confirm by clicking on the run button. In the following screenshot, you can see how the execution looks:

![](img/9337ac94537bf7e092e1ea6531dce8fd_683_0.png)

As you can see in the visual representation of the transformation, a green check highlights the steps executed with success. In the Logging panel, a detailed log trail is visible for your understanding. To confirm that everything has been executed with success, in the `data-integration` folder, you should find the `my_first_report.pdf` file containing the final rendering of the report.

If you have any kind of problem developing the example or you are too lazy to develop it, in the https://github.com/fcorti/pentaho-8-reporting-for-java-developers repository, specifically, in the `Chapter 16 - Using reports in Pentaho Data Integration` folder, you can find `my first transformation.ktr` introduced here. To install it in the Pentaho Data Integration, simply download it into the `data-integration` folder and open it as a regular file, using Spoon.

## Executing the transformation using Pan

To complete the example, let's see how to run the transformation from the command line using the pan script, with the purpose of scheduling the execution regularly using crontab (https://en.wikipedia.org/wiki/Cron).

Following the description given earlier in the chapter, open a Terminal and move into the data-integration folder, where you should find my first transformation.ktr, together with the my_first_report.prpt file. Type the following command and press the Enter button to launch its execution: ./pan.sh -file:"my first transformation.ktr"

In the following screenshot, you can see the result of the execution in the Terminal, as an example:

![](img/9337ac94537bf7e092e1ea6531dce8fd_684_0.png)

Scheduling the command using crontab or any other scheduler should be straightforward now, in accordance with your operating system.

## Summary

In this chapter, you learned how to use an existing Pentaho report within the Pentaho Data Integration, also known as Kettle, delivering powerful ETL capabilities. In this chapter, you were introduced to the basics of the Pentaho Data Integration, as well as provided with a description of how to install it and use Pentaho reports with the Pentaho Reporting output step. Afterwards, you learnt about Spoon client and about how to execute a job/transformation at command line, and you saw how to develop a transformation using Pentaho reports. Finally, you went through the execution of a transformation using Pan.

Now that you have read this chapter, you should feel comfortable with Pentaho Data Integration and all the basic features for managing Pentaho reports. This is an advanced task for developers and information technologists, to manage ETL batches for their future projects.

This chapter closes the section of the book dedicated to the use of Pentaho reports in other components of the Pentaho suite. In the next chapter, you will go through the final topics, just to add the latest details to your expertise.

## Pentaho Reporting Nightly Build and Support

This last chapter contains two main topics, that will hopefully be useful for completing your expertise in Pentaho Reporting.

The first topic is about downloading, building, installing, and running the latest version of Pentaho Report Designer, usually called **nightly build**. In the same way, you will learn how to use the nightly build in your Java projects; to try the latest version with the latest enhancements. This topic is strictly for developers and will enable you to access the latest version of Pentaho Reporting for evaluation purposes.

In the last topic of this book, you will go through all the ways you may have to find documentation, resources, support, meet the community, and so on as a new developer and expert into Pentaho Reporting. This topic is for developers and information technologists, and should help you to get support and help during your first project and future challenges.

The chapter (and the book) ends by sharing a thought on what to do next, after you have received all the benefits of this open source project and community, and you will become a real expert, with a lot of things to share with all the newbies on Pentaho Reporting.

## Pentaho Report Designer nightly build

In the previous chapters, you learnt everything about Pentaho Reporting 8.0 in its stable release. Stable releases can be downloaded as described in Chapter 2, *Getting Started with Report Designer*, but being an open source project, Pentaho Reporting's source code is available on a public repository hosted on GitHub. For future reference, the Pentaho Reporting's project is stored at https://github.com/pentaho/pentaho-reporting.

In the public repository, you can find all the libraries, source code, and everything that is required to understand how Pentaho Reporting works and is developed. Looking at the repository, there are several branches, as usual. The master branch hosts the daily commits of the Pentaho team. Of course, the source code cannot be guaranteed to be stable, but it's definitely the most recent version of the software.

In this section, you are going to learn how to download the latest version of the source code and how to build it, creating the so called nightly build. Using the Pentaho Reporting nightly build can be relevant for two main reasons: if you want to see the latest features in advance, without waiting for the stable release; and if you want to fork it, adding (or customizing) the official source code.

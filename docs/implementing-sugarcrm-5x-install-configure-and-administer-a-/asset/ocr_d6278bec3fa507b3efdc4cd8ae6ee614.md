Installation can sometimes be a challenge due to its size in comparison to other modules, but those issues are usually resolved by adjusting one or more of the following PHP.INI settings:

+   - upload_max_filesize
+   - post_max_size
+   - max_execution_time

Values of 50M, 50M, and 300 respectively, should suffice. Remember, if you need to adjust your settings, do not forget to restart your web service to apply the modifications. Once installed, ZuckerReports offers many powerful capabilities.

First and foremost, it offers a tool that allows you to create a variety of reports based on any of the modules – default or custom – included in your SugarCRM system. This is accomplished through a simple interface that most SugarCRM users will easily adapt to.

Reports are created by first producing a template that defines parameters, such as the target module, fields, filters, and other report parameters. To get a feel for the process, let us create a report that lists all of the opportunities in SugarCRM.

Our first step will be to access the ZuckerReports module. You can access it by selecting the **ZuckerReports** tab from the main navigation area within SugarCRM. Clicking on it should display a screen similar to the following:

![](bbox=[0.225, 0.528, 0.811, 0.774])